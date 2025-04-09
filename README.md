لبناء تطبيق بيع وشراء السيارات، يمكن تقسيم العمل إلى عدة مكونات رئيسية. سأقدم لك بعض الأكواد الأساسية التي يمكنك استخدامها كنقطة انطلاق. سنحتاج إلى واجهات لعرض السيارات، إضافة سيارات جديدة، وحذفها، بالإضافة إلى قاعدة بيانات لحفظ المعلومات.

1. واجهة المستخدم (UI) باستخدام HTML وCSS:

هذه الواجهة تسمح للمستخدمين بعرض السيارات المتاحة وإضافة سيارات جديدة.

<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>تطبيق بيع وشراء السيارات</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <header>
        <h1>تطبيق بيع وشراء السيارات</h1>
    </header>

    <section id="cars-list">
        <h2>السيارات المعروضة للبيع</h2>
        <ul id="car-items">
            <!-- هنا ستظهر السيارات المتاحة -->
        </ul>
    </section>

    <section id="add-car">
        <h2>إضافة سيارة جديدة</h2>
        <form id="car-form">
            <input type="text" id="car-name" placeholder="اسم السيارة" required>
            <input type="text" id="car-model" placeholder="طراز السيارة" required>
            <input type="number" id="car-price" placeholder="السعر" required>
            <button type="submit">إضافة السيارة</button>
        </form>
    </section>

    <script src="app.js"></script>
</body>
</html>

2. التصميم باستخدام CSS:

يتم تصميم الواجهة باستخدام بعض الأنماط البسيطة.

body {
    font-family: Arial, sans-serif;
    direction: rtl;
    background-color: #f4f4f9;
    padding: 20px;
}

header {
    text-align: center;
}

h1 {
    color: #333;
}

section {
    margin-top: 30px;
}

form input, form button {
    padding: 10px;
    margin: 5px;
    width: 100%;
}

button {
    background-color: #4CAF50;
    color: white;
    border: none;
    cursor: pointer;
}

button:hover {
    background-color: #45a049;
}

3. منطق JavaScript للتفاعل مع المستخدم:

هذا السكربت يتعامل مع إضافة سيارة جديدة وعرضها.

document.getElementById("car-form").addEventListener("submit", function (event) {
    event.preventDefault();

    // الحصول على القيم المدخلة
    let carName = document.getElementById("car-name").value;
    let carModel = document.getElementById("car-model").value;
    let carPrice = document.getElementById("car-price").value;

    // إنشاء عنصر جديد لعرض السيارة
    let carItem = document.createElement("li");
    carItem.innerHTML = `اسم السيارة: ${carName}, الطراز: ${carModel}, السعر: ${carPrice} ريال`;

    // إضافة العنصر إلى القائمة
    document.getElementById("car-items").appendChild(carItem);

    // مسح الحقول بعد الإضافة
    document.getElementById("car-form").reset();
});

4. تخزين البيانات في قاعدة بيانات باستخدام Node.js و MongoDB:

4.1 تنصيب الأدوات:

npm init -y
npm install express mongoose body-parser

4.2 إعداد الخادم باستخدام Express و MongoDB:

const express = require('express');
const mongoose = require('mongoose');
const bodyParser = require('body-parser');

// إعداد التطبيق
const app = express();
app.use(bodyParser.json());

// الاتصال بقاعدة البيانات
mongoose.connect('mongodb://localhost/car_shop', { useNewUrlParser: true, useUnifiedTopology: true });

// إنشاء نموذج للسيارة
const Car = mongoose.model('Car', {
    name: String,
    model: String,
    price: Number
});

// إضافة سيارة جديدة
app.post('/add-car', (req, res) => {
    const { name, model, price } = req.body;
    const newCar = new Car({ name, model, price });
    newCar.save()
        .then(() => res.status(201).send('تم إضافة السيارة'))
        .catch(err => res.status(400).send('فشل في إضافة السيارة: ' + err));
});

// عرض السيارات المتاحة
app.get('/cars', (req, res) => {
    Car.find()
        .then(cars => res.json(cars))
        .catch(err => res.status(400).send('خطأ في استرجاع البيانات: ' + err));
});

// بدء الخادم
app.listen(3000, () => {
    console.log('الخادم يعمل على المنفذ 3000');
});

5. التفاعل مع الواجهة الأمامية باستخدام JavaScript:

تحديث القائمة لعرض السيارات المتاحة من قاعدة البيانات.

window.onload = function() {
    // استرجاع السيارات من الخادم
    fetch('http://localhost:3000/cars')
        .then(response => response.json())
        .then(data => {
            const carList = document.getElementById('car-items');
            data.forEach(car => {
                let carItem = document.createElement('li');
                carItem.innerHTML = `اسم السيارة: ${car.name}, الطراز: ${car.model}, السعر: ${car.price} ريال`;
                carList.appendChild(carItem);
            });
        })
        .catch(error => console.log('خطأ في تحميل البيانات: ', error));
};

6. تشغيل التطبيق:

1. تأكد من تشغيل قاعدة البيانات MongoDB.


2. شغّل تطبيق Express باستخدام node app.js.


3. افتح ملف index.html في المتصفح.



ملاحظات:

هذا المثال يعتمد على MongoDB لتخزين البيانات. إذا كنت ترغب في استخدام قاعدة بيانات أخرى، يمكن تعديل الكود accordingly.

يمكن تحسين التطبيق بإضافة خصائص مثل فلاتر البحث، تحديد الموقع الجغرافي للسيارات، وغيرها من الوظائف.


هل تحتاج إلى مزيد من التفاصيل حول جانب معين من التطبيق؟

