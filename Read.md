## File: `backend/dataAccessLayer/category-controller.js`

``` javascript

const GetCategories = (request, response) => {
    try {
        let query = `SELECT 
                        A.category_id AS 'CategoryId',
                        A.department_id AS 'DepartmentId',
                        A.name AS 'Name',
                        A.description AS 'Description' 
                    FROM category A 
                    ORDER BY department_id ASC`; // query database to get all the departments

        // execute query
        db.query(query, (err, result) => {
           if (err != null) response.status(500).send({ error: error.message });

           return response.json(result);
       });
    } catch (error) {
        if (error != null) response.status(500).send({ error: error.message });
    }
} 

const category = {
    GetCategories
};

module.exports = category; 





## File: `backend/dataAccessLayer/customer-controller.js`

``` javascript
const  firebase = require("firebase/app");
const auth = require("firebase/auth");

var firebaseConfig = {
    apiKey: "AIzaSyBZTAjwO9-76Y-0sgJYxlrKF3J3k3qiuos",
    authDomain: "tshirt-shop-900ac.firebaseapp.com",
    databaseURL: "https://tshirt-shop-900ac.firebaseio.com",
    projectId: "tshirt-shop-900ac",
    storageBucket: "",
    messagingSenderId: "931749864223",
    appId: "1:931749864223:web:4b70ad4687b1063e"
  };
// Initialize Firebase
firebase.initializeApp(firebaseConfig);

const RegisterCustomer = (request, response) => {
    try {

        let params = request.body;

        firebase.auth()
                .createUserWithEmailAndPassword(params.Email, params.Password)
                .then(function(res){
                    let query = `INSERT INTO customer
                    (address_1, address_2, city, country, credit_card, day_phone, email, eve_phone, mob_phone, name, password, postal_code, region, shipping_region_id)
                    values
                    (
                        '${params.AddressOne}', 
                        '${params.AddressTwo}', 
                        '${params.Town}', 
                        '${params.Country}', 
                        '${params.CreditCard}', 
                        '', 
                        '${params.Email}', 
                        '', 
                        '${params.Mobile}', 
                        '${params.FirstName}', 
                        '', 
                        '${params.ZipCode}', 
                        '',
                        ${params.RegionId});`; // query database to get all the  Shipping Regions
            
                    // execute query
                    db.query(query, (err, result) => {
                       if (err != null) response.status(500).send({ error: err.message });
            
                       return response.json(true);
                   });
                })
                .catch(function(error) {
                    // Handle Errors here.
                    var errorCode = error.code;
                    var errorMessage = error.message;
                    return response.status(500).send({ error: error.message });
                    // ...
                });
    } catch (error) {
        if (error != null) response.status(500).send({ error: error.message });
    }
};

// validate login details and sign in
const AuthenticateLogin = (request, response) => {
    try {
        let params = request.body;
        SignInRegular(params.Username, params.Password)
            .then((res) => {
                let query = `SELECT 
                    A.email AS 'Email',
                    A.password AS 'Password',
                    A.address_1 AS 'AddressOne',
                    A.address_2 AS 'AddressTwo',
                    A.city AS 'Town',
                    A.country AS 'Country',
                    A.credit_card AS 'CreditCard',
                    A.customer_id AS 'CustomerId',
                    A.mob_phone AS 'Mobile',
                    A.name AS 'FullName',
                    A.postal_code AS 'ZipCode',
                    A.shipping_region_id AS 'RegionId'
                    FROM  customer A
                    WHERE A.email = '${params.Username}';`; // query database to get all the  Shipping Regions

                // execute query
                db.query(query, (err, result) => {
                    if (err != null) response.status(500).send({ error: err.message });
                    return response.json(result);
                });
            })
            .catch((error) => {
                return response.status(500).send({ error: error.message });
            });
    } catch (error) {
        if (error != null) response.status(500).send({ error: error.message });
    }
};

const Logout = (request, response) => {
    try {
        firebase.auth().signOut().then(res => {
            return response.json(res);
        })
    } catch (error) {
        if (error != null) response.status(500).send({ error: error.message });
    }
};

// sign in using firebase authentication
const SignInRegular = (email, password) => {
    return firebase.auth().signInWithEmailAndPassword(email, password);
}

const customer = {
    RegisterCustomer,
    AuthenticateLogin,
    Logout
};

module.exports = customer; 





## File: `backend/dataAccessLayer/department-controller.js`

``` javascript

const GetDepartments = (request, response) => {
    try {
        console.log(1)
        let query = `SELECT 
                        A.department_id AS 'DepartmentId',
                        A.name AS 'Name',
                        A.description AS 'Description' 
                    FROM department A 
                    ORDER BY department_id ASC`; // query database to get all the departments

        // execute query
        db.query(query, (err, result) => {
            console.log(2)
            if (err != null) response.status(500).send({ error: error.message });

            return response.json(result);
       });
    } catch (error) {
        console.log(2)
        if (error != null) response.status(500).send({ error: error.message });
    }
}

const departments = {
    GetDepartments
}

module.exports = departments; 





## File: `backend/dataAccessLayer/order-controller.js`

``` javascript
const nodemailer = require('nodemailer');

const CreateOrder = (request, response) => {
    try {

        const user = request.body.User;
        const cart = request.body.Cart;
        const remark = request.body.Remarks;
        const totalAmount = request.body.TotalAmount;

        const transporter = nodemailer.createTransport({
            service: 'gmail',
            auth: {
                user: 'ajdarkslayer@gmail.com',
                pass: 'Anjana@123'
            }
            // host: 'smtp.ethereal.email',
            // port: 587,
            // auth: {
            //     user: 'rebeka.dietrich54@ethereal.email',
            //     pass: 'eUN3GvKdfRgFseD4X8'
            // }
        });

        let mailOptions = {
            from: 'ajdarkslayer@gmail.com',
            to: `${user.Email}`,
            subject: "Congratulations! Your order placed succesfully.", // Subject line
            text: `Hello ${user.Name}`, // plain text body
            html: `<b>Hello  ${user.Name} </b>`
        }
        // send mail with defined transport object
        transporter.sendMail(mailOptions, function (error, info) {
            if (error) {
                console.log(error);
                return response.json(error);
            } else {
                console.log('Email sent: ' + info.response);

                let query = `INSERT INTO orders
                                (total_amount, created_on, shipped_on, status, comments, customer_id, auth_code, reference, shipping_id, tax_id)
                            VALUES
                            (
                                ${totalAmount}, 
                                CURDATE(), 
                                CURDATE(), 
                                1, 
                                '${remark}', 
                                ${user.CustomerId}, 
                                '', 
                                '', 
                                1, 
                                1
                            );`; //query database to get all the departments

                // execute query
                db.query(query, (err, result) => {
                    if (err != null) response.status(500).send({ error: error.message });
                    let values = [];
                    cart.forEach(element => {
                        let row = '';
                        row = `(
                            ${result.insertId},
                            ${element.ProductId},
                            '',
                            ${element.Quantity},
                            ${element.Price}
                        )`;
                        values.push(row);
                    });
                    let rows = values.toString();

                    let subQuery = `INSERT INTO order_detail
                                        (order_id, product_id, attributes, product_name, quantity, unit_cost)
                                    values ${rows};`; //query database to get all the departments

                    db.query(subQuery, (err, result) => {
                        if (err != null) response.status(500).send({ error: err.message });
                        return response.json(result);
                    });

                    return response.json(result);
                });
            }
        });


    } catch (error) {
        if (error != null) response.status(500).send({ error: error.message });
    }
};

const SendTestMail = async ()=> {
    let remark = 'test mail';

    let testAccount = await nodemailer.createTestAccount();

     // create reusable transporter object using the default SMTP transport
    let transporter = nodemailer.createTransport({
        service: 'gmail',
            auth: {
                user: 'ajdarkslayer@gmail.com',
                pass: 'Anjana@123' 
            }
    });

    // const transporter = nodemailer.createTransport({
    //     // service: 'gmail',
    //     // auth: {
    //     //     user: 'ajdarkslayer@gmail.com',
    //     //     pass: '*****'
    //     // }
    // });

    let mailOptions = {
        from: '"Dark Slayer 👻" <no-reply@dark.com>',
        to: 'ajgihan@gmail.com',
        subject: "Order Details", // Subject line
        text: "test purpose", // plain text body
        html: `<b>test account ${remark} </b>`
    }

    await transporter.sendMail(mailOptions, (error, info) => {
        if (error) {
            console.log(error);
            return response.json(error);
        } else {
            console.log('Email sent: ' + info.response);
        }
    });
};

const order = {
    CreateOrder,
    SendTestMail
};

module.exports = order; 





## File: `backend/dataAccessLayer/product-controller.js`

``` javascript

const GetProducts = (request, response) => {
    try {
        let query = `SELECT TOP 10
                            P.product_id AS 'ProductId',
                            P.name AS 'Name',
                            P.description AS 'Description',
                            P.price AS 'Price',
                            P.discounted_price AS 'DescountedPrice',
                            P.image AS 'PrimaryImage',
                            P.image_2 AS 'SecondaryImage',
                            P.thumbnail AS 'Thumbnail',
                            P.display AS 'Display',
                            P.category_id AS 'CategoryId',
                            P.department_id AS 'DepartmentId'
                    FROM product P, category C, product_category PC
                    WHERE P.product_id = PC.product_id 
                        AND C.category_id = PC.category_id;`; // query database to get all the departments'

        let productCountQuery = `SELECT COUNT(P.product_id) AS 'ProductCount'
                    FROM 
                        product P, 
                        category C, 
                        department D, 
                        product_category PC
                    WHERE P.product_id = PC.product_id 
                        AND C.category_id = PC.category_id
                        AND C.department_id = D.department_id
                    ${filterDepartment} ${filterCategory};`;

        // execute query
        db.query(query + productCountQuery, [1, 2], (err, result) => {
            if (err != null){
                response.status(500).send({ error: err.message });
            }

            let resultSet = {
                Products: result[0], 
                ProductCount: result[1]
            }
            // get product attributes
            let productIdList = [];
            resultSet.Products.forEach((element, index) => {
                 productIdList.push(element.ProductId);
            });

            let productlistString = productIdList.toString();

            let query = `SELECT 
                A.name AS 'AttributeName',
                A.attribute_id AS 'AttributeId',
                AV.attribute_value_id AS 'AttributeValueId',
                AV.value AS 'AttributeValue',
                PA.product_id AS 'ProductId'
            FROM attribute_value AV
            INNER JOIN attribute A
                    ON AV.attribute_id = A.attribute_id
            INNER JOIN product_attribute PA
                    ON PA.attribute_value_id = AV.attribute_value_id
            WHERE AV.attribute_value_id IN
                    (SELECT attribute_value_id
                    FROM   product_attribute
                    WHERE  product_id in (${productlistString}))
            ORDER BY A.name`;

            // execute query
            db.query(query, (err, result) => {
                if (err != null){
                    response.status(500).send({ error: err.message });
                }

                resultSet.Products.forEach((element,index) => {
                    var aaa = result.filter(a => a.ProductId == element.ProductId);
                    resultSet.Products[index]['Size'] = aaa.filter(a => a.AttributeId == 1);
                    resultSet.Products[index]['Color'] = aaa.filter(a => a.AttributeId == 2);
                });
                return response.json(resultSet);
            });

           return response.json(result);
       });
    } catch (error) {
        if (error != null) response.status(500).send({ error: error.message });
    }
};

const GetProductAttributes = (request, response) => {
    try {
        let query = 'CALL catalog_get_attribute_values(1);CALL catalog_get_attribute_values(2)'
        // execute query
        db.query(query, [1, 2], (err, result) => {
            if (err != null){
                response.status(500).send({ error: err.message });
               }
            return response.json({Size: result[0], Color: result[1]});
        });
    } catch (error) {
        
    }
};

const GetFilteredProducts = (request, response) => {
    try {
        let filterDepartment = (request.body.paging.DepartmentId == 0) ? 'AND C.department_id = C.department_id' : `AND C.department_id = ${request.body.paging.DepartmentId}`;
        let filterCategory = (request.body.paging.CategoryId == 0) ? 'AND C.category_id = C.category_id' : `AND C.category_id = ${request.body.paging.CategoryId}`;
        let filterSearchString = ''; 
        request.body.paging.SearchString = (request.body.paging.SearchString == undefined) ? '': request.body.paging.SearchString;
        
        if(request.body.paging.SearchString == ''){
            filterSearchString = `P.name like '%%' OR P.description like '%%'`;
        } else if(request.body.paging.IsAllWords) {
            let words = request.body.paging.SearchString.split(' ');
            let likeQuery = [];
            words.forEach(element => {
                likeQuery.push(`P.name like '%${element}%' OR P.description like '%${element}%'`);
            });
            filterSearchString = likeQuery.join(' OR ');
        } else{
            filterSearchString = `P.name like '%${request.body.paging.SearchString}%' 
                                 OR P.description like '%${request.body.paging.SearchString}%'`;
        }
        let query = `SELECT 
                        P.product_id AS 'ProductId',
                        P.name AS 'Name',
                        P.description AS 'Description',
                        P.price AS 'Price',
                        P.discounted_price AS 'DescountedPrice',
                        P.image AS 'PrimaryImage',
                        P.image_2 AS 'SecondaryImage',
                        P.thumbnail AS 'Thumbnail',
                        P.display AS 'Display',
                        C.category_id AS 'CategoryId',
                        C.department_id AS 'DepartmentId',
                        D.name AS 'DepartmentName',
                        C.name AS 'CategoryName'
                    FROM product P, category C, department D, product_category PC
                    WHERE P.product_id = PC.product_id 
                        AND C.category_id = PC.category_id
                        AND C.department_id = D.department_id
                        ${filterDepartment} ${filterCategory}
                        AND (${filterSearchString})
                    LIMIT ${request.body.paging.PageNumber}, ${request.body.paging.PageSize};`; // query database to get all the departments

        let productCountQuery = `SELECT COUNT(P.product_id) AS 'ProductCount'
                                FROM product P, category C, department D, product_category PC
                                WHERE P.product_id = PC.product_id 
                                    AND C.category_id = PC.category_id
                                    AND C.department_id = D.department_id
                                    ${filterDepartment} ${filterCategory}
                                    AND (${filterSearchString});`; // query database to get related product count
        // execute query
        db.query(query + productCountQuery, [1, 2], (err, result) => {
           if (err != null){
            return response.status(500).send({ error: err.message });
           }
            let resultSet = {
               Products: result[0], 
               ProductCount: result[1]
            }
            if(resultSet.Products.length == 0){
                return response.json(resultSet);
            }
           // get product attributes
            let productIdList = [];
            resultSet.Products.forEach((element, index) => {
                productIdList.push(element.ProductId);
            });
            let productlistString = productIdList.toString();

            let query = `SELECT 
                A.name AS 'AttributeName',
                A.attribute_id AS 'AttributeId',
                AV.attribute_value_id AS 'AttributeValueId',
                AV.value AS 'AttributeValue',
                PA.product_id AS 'ProductId'
            FROM attribute_value AV
            INNER JOIN attribute A
                    ON AV.attribute_id = A.attribute_id
            INNER JOIN product_attribute PA
                    ON PA.attribute_value_id = AV.attribute_value_id
            WHERE AV.attribute_value_id IN
                    (SELECT attribute_value_id
                    FROM   product_attribute
                    WHERE  product_id in (${productlistString}))
            ORDER BY A.name`;

            // execute query
            db.query(query, (err, result) => {
                if (err != null){
                    response.status(500).send({ error: err.message });
                }

                resultSet.Products.forEach((element,index) => {
                    var aaa = result.filter(a => a.ProductId == element.ProductId);
                    resultSet.Products[index]['Size'] = aaa.filter(a => a.AttributeId == 1).sort(function(a, b){return a.AttributeValueId - b.AttributeValueId});
                    resultSet.Products[index]['Color'] = aaa.filter(a => a.AttributeId == 2).sort(function(a, b){return a.AttributeValueId - b.AttributeValueId});
                });
                return response.json(resultSet);
            });

       });
    } catch (err) {
        if (err != null) {
            response.status(500).send({ error: err });
        }
    }
};

const GetProductDetailsById = (request, response) => {
    try {
        let query = `SELECT 
                        P.product_id AS 'ProductId',
                        P.name AS 'Name',
                        P.description AS 'Description',
                        P.price AS 'Price',
                        P.discounted_price AS 'DescountedPrice',
                        P.image AS 'PrimaryImage',
                        P.image_2 AS 'SecondaryImage',
                        P.thumbnail AS 'Thumbnail',
                        P.display AS 'Display',
                        C.category_id AS 'CategoryId',
                        C.department_id AS 'DepartmentId',
                        D.name AS 'DepartmentName',
                        C.name AS 'CategoryName'
                    FROM 
                        product P, 
                        category C, 
                        department D, 
                        product_category PC
                    WHERE P.product_id = PC.product_id 
                    AND C.category_id = PC.category_id
                    AND C.department_id = D.department_id
                    AND P.product_id = ${request.query.productId}`; // query database to get all the departments

        // execute query
        db.query(query ,(err, result) => {
            if (err != null){
                response.status(500).send({ error: err.message });
            }
            let productDetails = result[0];
            let subquery = `SELECT 
                            A.name AS 'AttributeName',
                            A.attribute_id AS 'AttributeId',
                            AV.attribute_value_id AS 'AttributeValueId',
                            AV.value AS 'AttributeValue',
                            PA.product_id AS 'ProductId'
                        FROM attribute_value AV
                        INNER JOIN attribute A
                                ON AV.attribute_id = A.attribute_id
                        INNER JOIN product_attribute PA
                                ON PA.attribute_value_id = AV.attribute_value_id
                        WHERE PA.product_id = ${request.query.productId}
                        ORDER BY A.name`;

            // execute query
            db.query(subquery, (err, results) => {
                if (err != null){
                    response.status(500).send({ error: err.message });
                }

                productDetails['Size'] = results.filter(a => a.AttributeId == 1).sort(function(a, b){return a.AttributeValueId - b.AttributeValueId});
                productDetails['Color'] = results.filter(a => a.AttributeId == 2).sort(function(a, b){return a.AttributeValueId - b.AttributeValueId});

                return response.json(productDetails);
            });

       });
    } catch (err) {
        if (err != null) {
            response.status(500).send({ error: err });
        }
    }
};

const product = {
    GetProducts,
    GetProductAttributes,
    GetFilteredProducts,
    GetProductDetailsById
};

module.exports = product; 





## File: `backend/dataAccessLayer/shipping-controller.js`

``` javascript
const GetShippingRegions = (request, response) => {
    try {
        let query = `SELECT 
                        A.shipping_region_id AS 'RegionId',
                        A.shipping_region AS 'Region'
                    FROM shipping_region A 
                    ORDER BY shipping_region_id ASC`; // query database to get all the  Shipping Regions

        // execute query
        db.query(query, (err, result) => {
           if (err != null) response.status(500).send({ error: error.message });

           return response.json(result);
       });
    } catch (error) {
        if (error != null) response.status(500).send({ error: error.message });
    }
};

const shipping = {
    GetShippingRegions
};

module.exports = shipping;  





## File: `backend/routes/category.js`

``` javascript

    
const express = require('express');
const router = express.Router();
const { GetCategories } = require('../dataAccessLayer/category-controller');

//get all departments
router.get('/getCategories', GetCategories);

module.exports = router; 





## File: `backend/routes/customer.js`

``` javascript

    
const express = require('express');
const router = express.Router();
const {
    AuthenticateLogin,
    RegisterCustomer,
    Logout
} = require('../dataAccessLayer/customer-controller');

//register new user
router.post('/addNewCustomer', RegisterCustomer);

//get username and password
router.post('/authenticateLogin', AuthenticateLogin);

// Logout from the system
router.get('/logout', Logout)

module.exports = router; 





## File: `backend/routes/department.js`

``` javascript

    
const express = require('express');
const router = express.Router();
const { GetDepartments } = require('../dataAccessLayer/department-controller');

//get all departments
router.get('/getDepartments', GetDepartments);

module.exports = router; 





## File: `backend/routes/order.js`

``` javascript
// const dotenv = require('dotenv').config();
const express = require('express');
const router = express.Router();
const { CreateOrder, SendTestMail } = require('../dataAccessLayer/order-controller');

//get all departments
router.post('/submitOrder', CreateOrder);
router.get('/sendTestMail', SendTestMail);

module.exports = router; 





## File: `backend/routes/product.js`

``` javascript

    
const express = require('express');
const router = express.Router();
const { 
    GetProducts,
    GetProductAttributes,
    GetFilteredProducts,
    GetProductDetailsById
 } = require('../dataAccessLayer/product-controller');

//get all Product
router.get('/getProducts', GetProducts);

// Get product related attributes
router.get('/getProductAttributes', GetProductAttributes)

//get all Filtered Products
router.post('/getFilteredProducts', GetFilteredProducts);

// get product details by product id
router.get('/getProductDetails', GetProductDetailsById)


module.exports = router; 





## File: `backend/routes/shipping.js`

``` javascript

    
const express = require('express');
const router = express.Router();
const { GetShippingRegions } = require('../dataAccessLayer/shipping-controller');

//get all Shipping Regions
router.get('/getShippingRegions', GetShippingRegions);

module.exports = router; 





## File: `backend/server.js`

``` javascript
const express = require('express');
const fileUpload = require('express-fileupload');
const bodyParser = require('body-parser');
const mysql = require('mysql');
const path = require('path');
const app = express();

// const {getHomePage} = require('./routes/index');
// const {addPlayerPage, addPlayer, deletePlayer, editPlayer, editPlayerPage} = require('./routes/player');
const port = process.env.PORT || 8080; 

// create connection to database
// the mysql.createConnection function takes in a configuration object which contains host, user, password and the database name.
//mysql://ba9565c7d6951a:5230e113@us-cdbr-iron-east-02.cleardb.net/heroku_f254459ae2fc71b?reconnect=true 
const db_config = {
    host: 'us-cdbr-iron-east-02.cleardb.net',
    user: 'b22789322cfb92', // your database username
    password: 'b7499b6f', // your database password
    database: 'heroku_fb4bfcaea035c93',  // FYI export the tshirtshop.sql to this database
    multipleStatements: true
}

var connection;

function handleDisconnect() {
  connection = mysql.createConnection(db_config); // Recreate the connection, since
                                                  // the old one cannot be reused.

  connection.connect(function(err) {              // The server is either down
    if(err) {                                     // or restarting (takes a while sometimes).
      console.log('error when connecting to db:', err);
      setTimeout(handleDisconnect, 2000);                                               
    }    
    /*
        We introduce a delay before attempting to reconnect,
        to avoid a hot loop, and to allow our node script to
        process asynchronous requests in the meantime.
        If you're also serving http, display a 503 error.
    */                                                                            
    global.db = connection;
    console.log(`Connected to database ${db_config.host} >> ${db_config.database}`);    
  });                                                                                   
                                          
  
  /*
    Connection to the MySQL server is usually
    lost due to either server restart, or a
    connnection idle timeout (the wait_timeout
    server variable configures this)
  */
  connection.on('error', function(err) {
    console.log('db error', err);
    handleDisconnect();    
  });
}

handleDisconnect();



// configure middleware
app.set('port', process.env.port || port); // set express to use this port
// app.set('views', __dirname + '/views'); // set express to look in this folder to render our view
// app.set('view engine', 'ejs'); // configure template engine
app.use(bodyParser.urlencoded({ extended: false }));
app.use(bodyParser.json()); // parse form data client
app.use(express.static(path.join(__dirname, 'public'))); // configure express to use public folder
app.use(fileUpload()); // configure fileupload

app.use(function (request, response, next) {
    response.header("Access-Control-Allow-Origin", "*");
    response.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
  next();
});

// import routes
const departmentRoutes = require('./routes/department');
const categoryRoutes = require('./routes/category');
const productRoutes = require('./routes/product');
const shippingRoutes = require('./routes/shipping');
const customerRoutes = require('./routes/customer');
const orderRoutes = require('./routes/order');

app.get('/', function (request, response, next) {
    db.query("SELECT * FROM category", function (error, rows) {
        return response.json(rows);
    });
});

// set routes to api
app.use('/api/department', departmentRoutes);
app.use('/api/category', categoryRoutes);
app.use('/api/product', productRoutes);
app.use('/api/shipping', shippingRoutes);
app.use('/api/customer', customerRoutes);
app.use('/api/order', orderRoutes);

// set the app to listen on the port
app.listen(port, () => {
    console.log(`Server running on port: ${port}`);
}); 





## File: `frontend/README.md`

``` markdown
# Frontend

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 8.0.3.

## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The app will automatically reload if you change any of the source files.

## Code scaffolding

Run `ng generate component component-name` to generate a new component. You can also use `ng generate directive|pipe|service|class|guard|interface|enum|module`.

## Build

Run `ng build` to build the project. The build artifacts will be stored in the `dist/` directory. Use the `--prod` flag for a production build.

## Running unit tests

Run `ng test` to execute the unit tests via [Karma](https://karma-runner.github.io).

## Running end-to-end tests

Run `ng e2e` to execute the end-to-end tests via [Protractor](http://www.protractortest.org/).

## Further help

To get more help on the Angular CLI use `ng help` or go check out the [Angular CLI README](https://github.com/angular/angular-cli/blob/master/README.md).
"# Simple-E-Commerce-Application" 
 





## File: `frontend/e2e/protractor.conf.js`

``` javascript
// @ts-check
// Protractor configuration file, see link for more information
// https://github.com/angular/protractor/blob/master/lib/config.ts

const { SpecReporter } = require('jasmine-spec-reporter');

/**
 * @type { import("protractor").Config }
 */
exports.config = {
  allScriptsTimeout: 11000,
  specs: [
    './src/**/*.e2e-spec.ts'
  ],
  capabilities: {
    'browserName': 'chrome'
  },
  directConnect: true,
  baseUrl: 'http://localhost:4200/',
  framework: 'jasmine',
  jasmineNodeOpts: {
    showColors: true,
    defaultTimeoutInterval: 30000,
    print: function() {}
  },
  onPrepare() {
    require('ts-node').register({
      project: require('path').join(__dirname, './tsconfig.json')
    });
    jasmine.getEnv().addReporter(new SpecReporter({ spec: { displayStacktrace: true } }));
  }
}; 





## File: `frontend/karma.conf.js`

``` javascript
// Karma configuration file, see link for more information
// https://karma-runner.github.io/1.0/config/configuration-file.html

module.exports = function (config) {
  config.set({
    basePath: '',
    frameworks: ['jasmine', '@angular-devkit/build-angular'],
    plugins: [
      require('karma-jasmine'),
      require('karma-chrome-launcher'),
      require('karma-jasmine-html-reporter'),
      require('karma-coverage-istanbul-reporter'),
      require('@angular-devkit/build-angular/plugins/karma')
    ],
    client: {
      clearContext: false // leave Jasmine Spec Runner output visible in browser
    },
    coverageIstanbulReporter: {
      dir: require('path').join(__dirname, './coverage/frontend'),
      reports: ['html', 'lcovonly', 'text-summary'],
      fixWebpackSourcePaths: true
    },
    reporters: ['progress', 'kjhtml'],
    port: 9876,
    colors: true,
    logLevel: config.LOG_INFO,
    autoWatch: true,
    browsers: ['Chrome'],
    singleRun: false,
    restartOnFileChange: true
  });
};
 





## File: `frontend/src/app/app.component.html`

``` html
<router-outlet></router-outlet> 





## File: `frontend/src/app/components/account/account.component.html`

``` html
<br>
<br>

<div class="container mt-5">
    <div class="row">
        <div class="col-sm-4">
            <ul class="list-group">
                <li class="list-group-item hoverable">
                    <a href="#">Your Profile</a>
                </li>
                <li class="list-group-item hoverable">
                    <a href="#">Your Wishlist</a>
                </li>
                <li class="list-group-item hoverable">
                    <a href="#">Your Cart</a>
                </li>
            </ul>
        </div>
        <div class="col-sm-8">
            <form>
                <div class="row">
                    <img src="https://angular-shoppingcart.firebaseapp.com/assets/img/malecostume-512.png" [alt]="loggedUser.FullName"
                        style="width: 35%; height: 35%;">
                    <div class="col-md-8 mt-3">
                        <label for="userName">Name</label>
                        <p style="font-size: 15px;">{{loggedUser.FullName}}</p>
                    </div>
                    <div class="col-md-8 mb-3">
                        <label for="emailId">Email</label>
                        <p style="font-size: 15px;">{{loggedUser.Email}}</p>
                    </div>
                    <div class="col-md-8 mb-3">
                        <label for="emailId">Mobile</label>
                        <p style="font-size: 15px;">{{loggedUser.Mobile}}</p>
                    </div>
                    <div class="col-md-8 mb-3">
                        <label for="emailId">Country</label>
                        <p style="font-size: 15px;">{{loggedUser.Country}}</p>
                    </div>
                </div>
            </form>
        </div>
    </div>
</div> 





## File: `frontend/src/app/components/checkout/checkout-navbar/checkout-navbar.component.html`

``` html
<h3 class="text-center">{{ activatedRoute._routerState.snapshot.url}}</h3>

<div class="board-inner" id="status-buttons">
    <ul class="nav nav-tabs" id="myTab">
        <div class="liner"></div>

        <!-- circular user icon -->
        <li id="productsTab" style="margin: auto" *ngIf="activatedRoute._routerState.snapshot.url == '/checkout/review'">
            <a [routerLink]="['/checkouts', {outlets: {'checkOutlet': ['/']}}]" routerLinkActive="active"
                [routerLinkActiveOptions]="{ exact: true }" data-toggle="tab" title="Product Summary">
                <span class="round-tabs one">
                    <i class="fa fa-shopping-cart" aria-hidden="true"></i>
                </span>
            </a>
        </li>

        <!-- circular customer icon -->
        <li id="customerTab" style="margin: auto" *ngIf="activatedRoute._routerState.snapshot.url == '/checkout/customer-information'">
            <a [routerLink]="['/checkouts', {outlets: {'checkOutlet': ['/']}}]" routerLinkActive="active"
                [routerLinkActiveOptions]="{ exact: true }" data-toggle="tab" title="Product Summary">
                <span class="round-tabs two">
                    <i class="fa fa-user-tie" aria-hidden="true"></i>
                </span>
            </a>
        </li>

        <!-- circular ok icon -->
        <li id="resultTab" style="margin: auto" *ngIf="activatedRoute._routerState.snapshot.url == '/checkout/payment-information'">
            <a [routerLink]="['/checkouts', {outlets: {'checkOutlet': ['result']}}]" routerLinkActive="active"
                data-toggle="tab" title="Payment">
                <span class="round-tabs four">
                    <i class="fa fa-credit-card" aria-hidden="true"></i>
                </span>
            </a>
        </li>

    </ul>
    <div class="clearfix"></div>
</div> 





## File: `frontend/src/app/components/checkout/checkout.component.html`

``` html
<div>
    <header>
        <app-header #appHeader></app-header>

    </header>
    <main class="pt-5 mt-5">
        <section>
            <div class="container ">
                <div class="board">
                    <!-- Navigation Area (Circular Tabs) -->
                    <app-checkout-navbar></app-checkout-navbar>
                    <!-- End Navigation Area (Circular Tabs) -->

                    <!-- Content Area -->
                    <div class="tab-content">
                        <!-- Routed view  -->
                        <router-outlet></router-outlet>
                    </div>
                    <!-- End Content Area -->
                </div>

                <!-- For Debugging: show our formData as it is being typed -->
                <!-- <pre>{{ formData | json }}</pre> -->
            </div>
        </section>
    </main>
</div> 





## File: `frontend/src/app/components/checkout/customer-info/customer-info.component.html`

``` html
<div class="container mt-3">
    <div class="col-md order-md-1">
        <h4 class="mb-3">Billing Address</h4>
        <form class="needs-validation" #userDetailsForm="ngForm">
            <div class="row">
                <div class="col-md-6 mb-3">
                    <label for="firstName">Full name</label>
                    <input type="text" readonly class="form-control" id="" [(ngModel)]="customerInfor.FullName"
                        name="firstName">
                </div>
                <div class="col-md-6 mb-3">
                    <label for="email">Email
                    </label>
                    <input type="email" class="form-control" id="" [(ngModel)]="customerInfor.Email" name="email"
                        readonly>
                </div>
            </div>

            <div class="mb-3">
                <label for="address">Address</label>
                <input type="text" class="form-control" id="" placeholder="1234 Main St"
                    [(ngModel)]="customerInfor.AddressOne" name="address1" readonly>
            </div>

            <div class="mb-3">
                <label for="address2">Address 2
                </label>
                <input type="text" class="form-control" id="" placeholder="Apartment or suite"
                    [(ngModel)]="customerInfor.AddressTwo" name="address2" readonly>
            </div>

            <div class="row">
                <div class="col-md-5 mb-3">
                    <label for="country">Country</label>
                    <input type="text" class="form-control" id="" placeholder="Country"
                        [(ngModel)]="customerInfor.Country" name="address2" readonly>
                </div>
                <div class="col-md-4 mb-3">
                    <label for="state">State</label>
                    <input type="text" class="form-control" id="" placeholder="City or Town"
                        [(ngModel)]="customerInfor.Town" name="address2" readonly>
                </div>
                <div class="col-md-3 mb-3">
                    <label for="zip">Zip</label>
                    <input type="text" class="form-control" id="" placeholder="ZipCode"
                        [(ngModel)]="customerInfor.ZipCode" name="address2" readonly>
                </div>
            </div>
            <hr class="mb-4">

        </form>
    </div>
    <div class="float-right">
            <a class="btn btn-success " [routerLink]="['/checkout/payment-information']"
                >Next</a>
        </div>
</div> 





## File: `frontend/src/app/components/checkout/payment-info/payment-info.component.html`

``` html
<div class="row mt-2 mb-3">
    <div class="col-md-12">
        <div class="card ">
            <div class="card-header">
                <h3 class="text-xs-center">
                    <strong>Cart summary</strong>
                    <!-- <app-paypal-checkout [totalAmount]="400" class="float-right"></app-paypal-checkout> -->
                </h3>
            </div>
            <div class="card-block">
                <div class="table-responsive">
                    <table class="table table-sm">
                        <thead>
                            <tr>
                                <td><strong>Item Name</strong></td>
                                <td class="text-xs-center"><strong>Item Price</strong></td>
                                <td class="text-xs-center"><strong>Item Quantity</strong></td>
                                <td class="text-xs-right"><strong>Total</strong></td>
                            </tr>
                        </thead>
                        <tbody>
                            <tr *ngFor="let item of checkoutProducts">
                                <td>{{item.Name}}</td>
                                <td class="text-xs-center">${{item.Price}}</td>
                                <td class="text-xs-center">{{item.Quantity}}</td>
                                <td class="text-xs-right">${{item.Price * item.Quantity}}</td>
                            </tr>
                            <tr>
                                <td class="highrow"></td>
                                <td class="highrow"></td>
                                <td class="highrow text-xs-center"></td>
                                <td class="highrow text-xs-right"></td>
                            </tr>
                        </tbody>
                    </table>
                    <div class="row pb-5 px-4 bg-white shadow-sm w-100">
                            <div class="col-lg-6">
                                <div class="bg-light px-4 py-3 text-uppercase font-weight-bold">Instructions for seller</div>
                                <div class="p-4">
                                    <p class="font-italic mb-4">If you have some information for the seller you can leave them in the box below
                                    </p>
                                    <textarea name="" cols="30" rows="2" class="form-control" [(ngModel)]="remark"></textarea>
                                </div>
                            </div>
                            <div class="col-lg-6">
                                <div class="pr-4 pl-4-0">
                                    <ul class="list-unstyled mb-4">
                                        <li class="d-flex justify-content-between py-3 border-bottom"><strong class="text-muted">Order Subtotal
                                            </strong><strong>${{totalPrice.toFixed(2)}}</strong></li>
                                        <li class="d-flex justify-content-between py-3 border-bottom"><strong class="text-muted">Shipping and
                                                handling</strong><strong>$0.00</strong></li>
                                        <li class="d-flex justify-content-between py-3 border-bottom"><strong class="text-muted">Total</strong>
                                            <h5 class="font-weight-bold">${{totalPrice.toFixed(2)}}</h5>
                                        </li>
                                    </ul>
                                    <app-paypal-checkout [comments]="remark" [totalAmount]="totalPrice.toFixed(2)" class="w-100"></app-paypal-checkout>
                                    <!-- <a href="#" class="btn btn-dark rounded-pill py-2 btn-block">Procceed to checkout</a> -->
                                </div>
                            </div>
                        </div>
                </div>
            </div>
        </div>
    </div>
</div>

 





## File: `frontend/src/app/components/checkout/payment-info/paypal-checkout/paypal-checkout.component.html`

``` html
<ngx-paypal [config]="payPalConfig"></ngx-paypal> 





## File: `frontend/src/app/components/checkout/review/review.component.html`

``` html
<div class="container">
    <ul class="list-group mb-3">
        <li class="list-group-item d-flex lh-condensed" *ngFor="let product of checkoutProducts">
            <div>
                <img [src]="'/assets/product_images/' + product.Thumbnail" [alt]="product.Name" height="100">
            </div>
            <div style="padding: 10px" class="pl-5">
                <h6 class=" my-0"><strong>{{product.Name}}</strong></h6>
                <p class="m-0">{{product.Description}}</p>
                <strong class="">
                    <div class="price">
                        ${{product.Price}}
                    </div>
                </strong>
            </div>

        </li>
        <hr>
        <li class="list-group-item d-flex justify-content-between ">
            <span>Total (INR)</span>
            <strong class="price">${{totalPrice}}</strong>
        </li>

    </ul>
    <div class="float-right">
        <a class="btn btn-success " [routerLink]="['/checkout/customer-information']"
            routerLinkActive="active">Next</a>
    </div>

</div> 





## File: `frontend/src/app/components/home/filters/filters.component.html`

``` html
 <!-- Card -->
 <mdb-card class="m-0">
    <!--Card content-->
    <mdb-card-body>
       <!--Title-->
       <mdb-card-title>
          <h4>Search the Catalog</h4>
       </mdb-card-title>
       
       <!--Text-->
       <mdb-card-text>

          <div class="md-form">
              <input mdbInput type="text" id="form1" class="form-control" [(ngModel)]="searchString">
              <label for="form1" class="">Search Text</label>
            </div>
            <div class="form-check mb-2 mr-sm-2">
                <input mdbInput class="form-check-input" type="checkbox" id="inlineFormCheck" [(ngModel)]="searchForAllWords">
                <label class="form-check-label" for="inlineFormCheck">
                  Search for all words
                </label>
              </div>
       </mdb-card-text>

       <a mdbBtn color="primary" mdbWavesEffect (click)="onClickSearch()">Search</a>
    </mdb-card-body>
 </mdb-card>

 <!-- Card -->

 <!-- Card -->
 <mdb-card class="m-0 mt-4">
    <!--Card content-->
    <mdb-card-body>
       <!--Title-->
       <mdb-card-title>
          <h4>Choose a Department</h4>
       </mdb-card-title>
       
       <!--Text-->
       <mdb-card-text>

          <div class="list-group">
              <a class="list-group-item list-group-item-action flex-column align-items-start" 
                *ngFor="let item of departmentList;index as i"
                [ngClass]="{'active': selectedDepartment == item.DepartmentId}"
                (click)="onSelectDepartment(item)"
                >
                <div class="d-flex w-100 justify-content-between">
                  <h6 class="mb-1">{{item.Name}}</h6>
                  <!-- <small>3 days ago</small> -->
                </div>
              </a>
            </div>
       </mdb-card-text>
    </mdb-card-body>
 </mdb-card>

 <!-- Card -->

 
 <!-- Card -->
 <mdb-card class="m-0 mt-4">
    <!--Card content-->
    <mdb-card-body>
       <!--Title-->
       <mdb-card-title>
          <h4>Choose a Category</h4>
       </mdb-card-title>
       
       <!--Text-->
       <mdb-card-text>

          <div class="list-group">
              <a class="list-group-item list-group-item-action flex-column align-items-start"
                *ngFor="let item of filteredCategoryList"
                [ngClass]="{'active': selectedCategory == item.CategoryId}"
                (click)=onSelectCategory(item)>
                <div class="d-flex w-100 justify-content-between">
                  <h6 class="mb-1">{{item.Name}}</h6>
                  <!-- <small>3 days ago</small> -->
                </div>
              </a>
            </div>
       </mdb-card-text>
    </mdb-card-body>
 </mdb-card>

 <!-- Card --> 





## File: `frontend/src/app/components/home/home.component.html`

``` html
<div class="container">
    <div class="mt-4">
       <div class="row">
          <div class="col-4">
            <app-filters [productList]="productList"></app-filters>
          </div>
          <div class="col-8">

             <!-- Card -->
             <mdb-card class="m-0">
                <!--Card content-->
                <mdb-card-body>
                   <!--Title-->
                   <mdb-card-title>
                        
                   </mdb-card-title>

                   <!--Text-->
                   <mdb-card-text> 
                        <app-product-list #productList></app-product-list>
                   </mdb-card-text>
                </mdb-card-body>
             </mdb-card>

             <!-- Card -->
          </div>
       </div>

    </div>
 </div> 





## File: `frontend/src/app/components/home/product-list/product-card/product-card.component.html`

``` html
<div class="product-grid grid">
  <div class="product-image">
    <a [routerLink]="['product-details', product.ProductId]">
      <img class="pic-1" [src]="'/assets/product_images/' + product.PrimaryImage">
      <img class="pic-2" [src]="'/assets/product_images/' + product.PrimaryImage">
    </a>
    <ul class="social">
      <li><a [routerLink]="['product-details', product.ProductId]" data-tip="Quick View"><i class="fa fa-eye"></i></a>
      </li>
    </ul>
    <!-- <span class="product-new-label">New</span>
            <span class="product-discount-label">-10%</span> -->
  </div>
  <div class="product-content">
    <span class="product-shipping">{{product.DepartmentName}} - {{product.CategoryName}}</span>
    <h3 class="title">
      <a [routerLink]="['product-details', product.ProductId]"><strong>
          {{product.Name}}
        </strong>
      </a>
    </h3>
    <div class="price">
      {{product.Price}}
    </div>

    <div class="row">
      <div class="col-md-6 py-1 px-1">
        <strong>
          Size
        </strong>
        <select class="browser-default custom-select">
          <option [value]="item.AttributeId" *ngFor="let item of product.Size">{{item.AttributeValue}}</option>
        </select>

      </div>
      <div class="col-md-6 py-1 px-1">
        <strong>
          Colour
        </strong>
        <select class="browser-default custom-select">
          <option [value]="item.AttributeId" *ngFor="let item of product.Color">{{item.AttributeValue}}</option>
        </select>
      </div>
    </div>
    <!-- <button class="add-to-cart" (click)="onAddProductToCart()">ADD TO CART</button> -->
    <app-add-to-cart [productId]="product.ProductId" [colorId]="colorId" [sizeId]="sizeId" [isHomePage]="true">
    </app-add-to-cart>
  </div>
</div> 





## File: `frontend/src/app/components/home/product-list/product-list.component.html`

``` html
<!--Section: Products v.1-->
<section class="text-center pb-3 mb-5 mt-1">
    <!--Section description-->
    <p class="grey-text pb-1 font-weight-normal font-italic">
        We hope you have fun developing TShirtShop, the e-commerce store from begining PHP and MySQL E-commerce: from Novoice to Professional!
        We have the largest collection of t-shirt with postal stamps on Earth! Browse our departments and categories to find your favorite!
 
    </p>
    <nav aria-label="breadcrumb">
        <ol class="breadcrumb">
            <a href="#">{{departmentName}}</a>
            <a href="#" *ngIf="categoryName">&nbsp;<span class="black-text">>></span>&nbsp;{{categoryName}}</a>
        </ol>
        <ol class="breadcrumb search-string" *ngIf="searchString">
            <a href="#">Product contains any of these words</a>
            <ng-container *ngIf="allWords; else elseTemplate">
                    <a href="#" *ngIf="searchString">&nbsp;<span class="black-text">>></span>&nbsp;{{searchString.split(' ').toString()}}</a>
            </ng-container>
            <ng-template #elseTemplate>
                    <a href="#" *ngIf="searchString">&nbsp;<span class="black-text">>></span>&nbsp;{{searchString}}</a>  
            </ng-template>
        </ol>
    </nav>
          
    <!--Grid row-->
    <div class="row">
        <!--Grid column-->
        <app-product-card *ngFor="let product of productList" [productData]="product" class="col-md-4 col-sm-6 mb-3 row-eq-height"></app-product-card>
          <!--Grid column-->
    </div>
    <!--Grid row-->
  </section>
  <nav aria-label="Page navigation example" *ngIf="PRODUCT_COUNT > 10">
      <app-pagination
        (goPage)="goToPage($event)"
        (goNext)="onNext()"
        (goPrev)="onPrev()"
        [pagesToShow]="5"
        [page]="CURRENT_PAGE"
        [perPage]="PER_PAGE"
        [count]="PRODUCT_COUNT">
      </app-pagination>
    </nav>
  <!--Section: Products v.1--> 





## File: `frontend/src/app/components/layout/app-header/app-header.component.html`

``` html
<nav class="navbar fixed-top navbar-expand-lg navbar-light white scrolling-navbar">
  <div class="container">

    <!-- Brand -->
    <a class="navbar-brand waves-effect" [routerLink]="['/']">
      <img src="../../../../assets/tshirtshop.png" height="30" alt="">
    </a>

    <!-- Collapse -->
    <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent"
      aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>

    <!-- Links -->
    <div class="collapse navbar-collapse" id="navbarSupportedContent">
      <!-- Hi!
        <a class="nav-link waves-light  pink-text p-0" href="#">Sign In</a>
        or
        <a class="nav-link waves-light  pink-text p-0" href="#">Register</a> -->
      <!-- Left -->
      <ul class="navbar-nav mr-auto">
        <li class="nav-item">
          <a class="nav-link waves-effect" href="#"><strong>Hi</strong>
            <span class="sr-only">(current)</span>
          </a>
        </li>
        <ng-container *ngIf="isLogged; else elseTemplate">
          <li class="nav-item" mdbDropdown>
            <a class="nav-link waves-effect" [routerLink]="['/customer/my-account']">
              <i class="fas fa-user mr-1 pink-text"></i>
              <strong class="pink-text">{{user.FullName}}</strong>
            </a>
          </li>
          <li class="nav-item" mdbDropdown>
            <a class="nav-link waves-effect" (click)="onLogout()">
              <i class="fas fa-sign-in-alt mr-1 pink-text"></i>
              <strong class="pink-text">Logout</strong>
            </a>
          </li>
        </ng-container>
        <ng-template #elseTemplate>
          <li class="nav-item" mdbDropdown>
            <a class="nav-link waves-effect" [routerLink]="['/customer/login']">
              <strong class="pink-text">Sign In</strong>
            </a>
          </li>
          <li class="nav-item">
            <a class="nav-link waves-effect" >
              <strong>or</strong>
            </a>
          </li>
          <li class="nav-item">
            <a class="nav-link waves-effect" [routerLink]="['/customer/register']" target="_blank">
              <strong class="pink-text">Register</strong>
            </a>
          </li>
        </ng-template>


      </ul>

      <!-- Right -->
      <ul class="navbar-nav nav-flex-icons">
        <li class="nav-item" mdbDropdown>
          <a class="nav-link waves-effect dropdown-toggle" mdbDropdownToggle>
            <!-- [routerLink]="['shopping-cart']" -->
            <span class="badge red z-depth-1 mr-1"> {{itemCount}} </span>
            <i class="fas fa-shopping-cart"></i>
            <span class="clearfix d-none d-sm-inline-block"> Cart </span>
          </a>
          <app-small-cart class="dropdown-menu dropdown-cart mr-5 pb-0" *ngIf="itemCount != 0"></app-small-cart>
      </ul>
    </div>

  </div>
</nav> 





## File: `frontend/src/app/components/layout/app-header/small-cart/small-cart.component.html`

``` html
<div class="table-responsive">
    <table class="table">
      <tbody>
        <tr *ngFor="let product of cart">
          <th scope="row" class="border-0">
            <div class="p-2">
              <img [src]="'/assets/product_images/' + product.Thumbnail" alt="" width="70" class="img-fluid rounded shadow-sm">
              <div class="ml-3 d-inline-block align-middle">
                <h5 class="mb-0"> 
                    <a href="#" class="text-dark d-inline-block align-middle">{{product.Name}}</a>
                </h5>
                <span class="text-muted font-weight-normal font-italic d-block ml-2">
                        <span class="badge purple mr-1">{{product.DepartmentName}}</span>
                        <span class="badge blue mr-1">{{product.CategoryName}}</span>
                </span>
              </div>
            </div>
          </th>
          <td class="border-0 align-middle">
                <var class="price">${{(product.Price * product.Quantity).toFixed(2)}}</var> 
                <small class="text-muted">(${{product.Price.toFixed(2)}} each)</small> 
          </td>
          <td class="border-0 align-middle"><strong>{{product.Quantity}}</strong></td>
          <td class="border-0 align-middle"><a (click)="onRemoveProductsFromCart(product.ProductId)" class="text-dark"><i class="fa fa-trash"></i></a></td>
        </tr>
      </tbody>
    </table>
  </div>
  <li class="divider"></li>
  <li><a class="text-center btn" [routerLink]="[ '/cart/shopping-cart' ]"><strong>View Cart</strong></a></li> 





## File: `frontend/src/app/components/layout/layout.component.html`

``` html
<div id="app">
    <header>
       <app-header #appHeader></app-header>
 
    </header>
    <main>
         <router-outlet ></router-outlet>
    </main>
 </div>
  





## File: `frontend/src/app/components/login/login.component.html`

``` html
<section class="register-block">
  <div class="container">
    <div class="row">
      <div class="col-md-4 register-sec">
        <h2 class="text-center">Sign in</h2>
        <form class="register-form" [formGroup]="loginForm" (ngSubmit)="onSubmit()">
          <div class="form-group">
              <label for="username" class="text-uppercase">Email</label>
              <input type="text" 
              formControlName="username" 
              class="form-control" 
              [ngClass]="{ 'is-invalid': submitted && f.username.errors }" />
              <div *ngIf="submitted && f.username.errors" class="invalid-feedback">
                  <div *ngIf="f.username.errors.required">Email is required</div>
              </div>
            <!-- <label for="exampleInputEmail" class="text-uppercase">Email</label>
            <input type="email" class="form-control" placeholder=""> -->
          </div>
          <div class="form-group">
              <label for="password" class="text-uppercase">Password</label>
              <input type="password" 
              formControlName="password" 
              class="form-control" 
              [ngClass]="{ 'is-invalid': submitted && f.password.errors }" />
              <div *ngIf="submitted && f.password.errors" class="invalid-feedback">
                  <div *ngIf="f.password.errors.required">Password is required</div>
              </div>
            <!-- <label for="exampleInputPassword1" class="text-uppercase">Password</label>
            <input type="password" class="form-control" placeholder=""> -->
          </div>
          <div class="form-check">
            <button type="submit" class="btn btn-register float-right">Submit</button>
          </div>

        </form>
s
      </div>
      <div class="col-md-8 banner-sec">
      </div>
    </div>
  </div>
</section> 





## File: `frontend/src/app/components/order-confirmation/order-confirmation.component.html`

``` html
<div class="jumbotron text-xs-center">
    <h1 class="display-3">Thank You!</h1>
    <p class="lead">Your Payment has been recived to us. <strong>Please check your email</strong> </p>
    <hr>
    <p>
        Having trouble? <a href="">Contact us</a>
    </p>
    <p class="lead">
        <a class="btn btn-success " [routerLink]="['/']" role="button">Continue to homepage</a>
    </p>
</div>
         





## File: `frontend/src/app/components/product-details/product-details.component.html`

``` html
<!--Main layout-->
<main class="mt-5 pt-4">

  <div class="container dark-grey-text mt-5">
    <div class="row">
      <div class="col-sm-4">
        <div class="product-image">
          <div class="view hm-zoom z-depth-2" style="cursor: pointer">
            <div class="d-flex product-image pt-2">
              <img [src]="'/assets/product_images/' + product.PrimaryImage" *ngIf="product.PrimaryImage"
                class="img-fluid " [alt]="product.Name">
            </div>
            <div class="d-flex product-image pt-2 pb-2">
              <img [src]="'/assets/product_images/' + product.SecondaryImage" *ngIf="product.SecondaryImage"
                class="img-fluid " [alt]="product.Name">
            </div>
          </div>
          <div class="" style="margin-top:15px">
            <ul class="list-group mb-3">
              <li class="list-group-item d-flex justify-content-between lh-condensed">
                <div>
                  <h6 class="my-0">Product Price</h6>
                </div>
                <span class="text-muted" style="color:crimson !important">${{product.Price}}</span>
              </li>
            </ul>
          </div>
        </div>
      </div>
      <div class="col-sm-8">
        <div class="product-detail">
          <h5 class="product-head">Product Details</h5>
          <table class="table" cellspacing="0" style="max-height: 28px">
            <tbody>
              <tr>
                <th scope="row">Product Name</th>
                <td>{{product.Name}}</td>
              </tr>
              <tr>
                <th scope="row">Product Description</th>
                <td>{{product.Description}}</td>
              </tr>
              <tr>
                <th scope="row">Product Category</th>
                <td>
                  <a href="">
                    <span class="badge purple mr-1">{{product.DepartmentName}}</span>
                  </a>
                  <a href="">
                    <span class="badge blue mr-1">{{product.CategoryName}}</span>
                  </a>
                </td>
              </tr>
              <tr>
                <td>
                  <strong class="d-flex">
                    Color
                  </strong>
                  <select class="browser-default custom-select" *ngIf="product" style="width: 100px"
                    [(ngModel)]="colorId">
                    <option [value]="item.AttributeValueId" *ngFor="let item of product.Color"> {{item.AttributeValue}}
                    </option>
                  </select>
                </td>
                <td>
                  <strong class="d-flex">
                    Size
                  </strong>
                  <select class="browser-default custom-select" *ngIf="product" style="width: 100px"
                    [(ngModel)]="sizeId">
                    <option [value]="item.AttributeValueId" *ngFor="let item of product.Size">{{item.AttributeValue}}
                    </option>
                  </select>
                </td>
              </tr>
              <tr>
                <td colspan="2">
                  <app-add-to-cart class="d-flex" [productId]="product.ProductId" [colorId]="colorId" [sizeId]="sizeId"
                    [isHomePage]="false"></app-add-to-cart>
                </td>
              </tr>
            </tbody>
          </table>

        </div>
      </div>
    </div>
  </div>
</main> 





## File: `frontend/src/app/components/register/register.component.html`

``` html
<section class="register-block">
    <div class="container">
            
        <div class="row register-sec">
            <div class="col-md-12 register-sec pb-0 pt-0">
                    <h2 class="text-center">Register Now</h2>
            </div>
            <form class="register-form col-md-12" [formGroup]="registerForm" (ngSubmit)="onSubmit()">
                <div class="row">
                    <div class="col-md-6 ">
                        <div class="form-group ">
                            <label for="FirstName" class="text-uppercase">First Name</label>
                            <input type="text" class="form-control is-invalid" 
                            formControlName="FirstName" 
                            class="form-control" 
                            [ngClass]="{ 'is-invalid': submitted && f.FirstName.errors }" >
                            
                            <div *ngIf="submitted && f.FirstName.errors" class="invalid-feedback">
                                <div *ngIf="f.FirstName.errors.required">First Name is required</div>
                            </div>
                        </div>
                        <div class="form-group">
                            <label for="AddressOne" class="text-uppercase">Address Line 1</label>
                            <input type="text" class="form-control" 
                            formControlName="AddressOne" 
                            class="form-control" 
                            [ngClass]="{ 'is-invalid': submitted && f.AddressOne.errors }" >

                            <div *ngIf="submitted && f.AddressOne.errors" class="invalid-feedback">
                                <div *ngIf="f.AddressOne.errors.required">First Name is required</div>
                            </div>
                        </div>
                        <div class="form-group">
                            <label for="Town" class="text-uppercase">Town</label>
                            <input type="text" class="form-control" 
                            formControlName="Town" 
                            class="form-control" 
                            [ngClass]="{ 'is-invalid': submitted && f.Town.errors }" >

                             <div *ngIf="submitted && f.Town.errors" class="invalid-feedback">
                                <div *ngIf="f.Town.errors.required">First Name is required</div>
                            </div>
                        </div>
                        <div class="form-group">
                            <label for="RegionId" class="text-uppercase">Region</label>
                            <select id="" class="form-control" 
                            formControlName="RegionId" 
                            class="form-control" 
                            [ngClass]="{ 'is-invalid': submitted && f.RegionId.errors }" >
                                <option [value]="item.RegionId" *ngFor="let item of regions">{{item.Region}}</option>
                            </select>

                             <div *ngIf="submitted && f.RegionId.errors" class="invalid-feedback">
                                <div *ngIf="f.RegionId.errors.required">First Name is required</div>
                            </div>
                        </div>
                        <div class="form-group">
                            <label for="Mobile" class="text-uppercase">Mobile</label>
                            <input type="text" class="form-control" 
                            formControlName="Mobile" 
                            class="form-control" 
                            [ngClass]="{ 'is-invalid': submitted && f.Mobile.errors }" >

                            <div *ngIf="submitted && f.Mobile.errors" class="invalid-feedback">
                                    <div *ngIf="f.Mobile.errors.required">First Name is required</div>
                                </div>
                        </div>
                        <div class="form-group">
                            <label for="Email" class="text-uppercase">Email</label>
                            <input type="text" class="form-control" 
                            formControlName="Email" 
                            class="form-control" 
                            [ngClass]="{ 'is-invalid': submitted && f.Email.errors }" >

                            <div *ngIf="submitted && f.Email.errors" class="invalid-feedback">
                                    <div *ngIf="f.Email.errors.required">First Name is required</div>
                                </div>
                        </div>
                    </div>
                    <div class="col-md-6">
                        <div class="form-group">
                            <label for="LastName" class="text-uppercase">Last Name</label>
                            <input type="text" class="form-control" 
                            formControlName="LastName" 
                            class="form-control" 
                            [ngClass]="{ 'is-invalid': submitted && f.LastName.errors }" >

                            <div *ngIf="submitted && f.LastName.errors" class="invalid-feedback">
                                    <div *ngIf="f.LastName.errors.required">First Name is required</div>
                                </div>
                        </div>
                        <div class="form-group">
                            <label for="AddressTwo" class="text-uppercase">Address Line 2</label>
                            <input type="text" class="form-control" 
                            formControlName="AddressTwo" 
                            class="form-control" 
                            [ngClass]="{ 'is-invalid': submitted && f.AddressTwo.errors }">

                            <div *ngIf="submitted && f.AddressTwo.errors" class="invalid-feedback">
                                    <div *ngIf="f.AddressTwo.errors.required">First Name is required</div>
                                </div>
                        </div>
                        <div class="form-group">
                            <label for="Country" class="text-uppercase">Country</label>
                            <input type="text" class="form-control" 
                            formControlName="Country" 
                            class="form-control" 
                            [ngClass]="{ 'is-invalid': submitted && f.Country.errors }" >

                            <div *ngIf="submitted && f.Country.errors" class="invalid-feedback">
                                    <div *ngIf="f.Country.errors.required">First Name is required</div>
                                </div>
                        </div>
                        <div class="form-group">
                            <label for="ZipCode" class="text-uppercase">Post Cod/Zip</label>
                            <input type="text" class="form-control" 
                            formControlName="ZipCode" 
                            class="form-control" 
                            [ngClass]="{ 'is-invalid': submitted && f.ZipCode.errors }" >

                            <div *ngIf="submitted && f.ZipCode.errors" class="invalid-feedback">
                                    <div *ngIf="f.ZipCode.errors.required">First Name is required</div>
                                </div>
                        </div>
                        <div class="form-group">
                            <label for="CreditCard" class="text-uppercase">Credit Card Number</label>
                            <input type="text" class="form-control" 
                            formControlName="CreditCard" 
                            class="form-control" 
                            [ngClass]="{ 'is-invalid': submitted && f.CreditCard.errors }" >

                            <div *ngIf="submitted && f.CreditCard.errors" class="invalid-feedback">
                                    <div *ngIf="f.CreditCard.errors.required">First Name is required</div>
                                </div>
                        </div>
                        <div class="form-group">
                            <label for="Password" class="text-uppercase">Password</label>
                            <input type="password" class="form-control" 
                            formControlName="Password" 
                            class="form-control" 
                            [ngClass]="{ 'is-invalid': submitted && f.Password.errors }" >

                            <div *ngIf="submitted && f.Password.errors" class="invalid-feedback">
                                    <div *ngIf="f.Password.errors.required">First Name is required</div>
                                </div>
                        </div>
                        <div class="form-check">
                            <button type="submit" class="btn btn-register float-right">Submit</button>
                        </div>
                    </div>
                </div>
            </form>
            <!-- <div class="col-md-8 banner-sec">
            </div> -->
        </div>
    </div>
</section> 





## File: `frontend/src/app/components/shared/add-to-cart/add-to-cart.component.html`

``` html

<ng-container *ngIf="isHomePage; else elseTemplate">
        <input type="number" value="1" aria-label="Search" class="form-control d-none" style="width: 100px" [(ngModel)]="quantity" >
        <button class="add-to-cart" (click)="onAddProductToCart()">ADD TO CART</button>
</ng-container>
<ng-template #elseTemplate>
        <input type="number" value="1" aria-label="Search" class="form-control" style="width: 100px" [(ngModel)]="quantity" >
        <button class="btn btn-primary btn-md my-0 p" type="button" (click)="onAddProductToCart()">Add to cart
                <i class="fas fa-shopping-cart ml-1"></i>
            </button>
</ng-template>

 





## File: `frontend/src/app/components/shared/pagination/pagination.component.html`

``` html
<ul class="pagination pagination-circle pg-blue justify-content-center" *ngIf="count > 0">
  <li class="page-item" 
    [ngClass]="{ 'disabled': page === 1 || loading }"
    (click)="onPage(1)" >
      <a class="page-link" mdbWavesEffect>
        First
      </a>
  </li>
  <li class="page-item" 
    (click)="onPrev()" 
    [ngClass]="{ 'disabled': page === 1 || loading }">
      <a class="page-link" aria-label="Previous" mdbWavesEffect>
        <span aria-hidden="true">&laquo;</span>
        <span class="sr-only">Previous</span>
      </a>
  </li>
  <li class="page-item" 
    *ngFor="let pageNum of getPages()" 
    (click)="onPage(pageNum)"
    [ngClass]="{'active': pageNum === page, 'disabled': loading}">
      <a class="page-link" mdbWavesEffect>
          {{ pageNum }}
      </a>
  </li>
  <li class="page-item" 
    (click)="onNext(true)" 
    [ngClass]="{ 'disabled': lastPage() || loading }">
      <a class="page-link" aria-label="Next" mdbWavesEffect>
        <span aria-hidden="true">&raquo;</span>
        <span class="sr-only">Next</span>
      </a>
  </li>
  <li class="page-item"
    [ngClass]="{ 'disabled': page === totalPages() || loading }"
    (click)="onPage(totalPages())" >
      <a class="page-link" mdbWavesEffect>
        Last
      </a>
  </li>
</ul>
 





## File: `frontend/src/app/components/shopping-cart/shopping-cart.component.html`

``` html
<div class="px-4 px-lg-0 pt-5">
    <!-- For demo purpose -->
    
    <div class="pb-5">
        <div class="container">
          <div class="row" *ngIf="cart.length > 0">
            <div class="col-lg-12 p-5 bg-white rounded shadow-sm mb-5">
    
                <div class="card shopping-cart">
                    <div class="card-header bg-dark text-light">
                        <strong>
                                <a href="#" class="btn btn-md float-left">
                                  <i class="fa fa-shopping-cart" aria-hidden="true"></i>
                                  Shopping Cart
                                </a>
                                
                        </strong>
                        <a [routerLink]="['/']" class="btn btn-outline-info btn-md float-right">Continue shopping</a>
                        <div class="clearfix"></div>
                    </div>
                    <div class="card-body">
                            <!-- PRODUCT -->
                            <div class="row" *ngFor="let product of cart">
                                <div class="col-12 col-sm-12 col-md-2 text-center">
                                        <img class="img-responsive" [src]="'/assets/product_images/' + product.Thumbnail"  alt="product.Name"  height="80">
                                </div>
                                <div class="col-12 text-sm-center col-sm-12 text-md-left col-md-5">
                                    <h4 class="product-name"><strong>{{product.Name}}</strong></h4>
                                    <dl class="param param-inline small">
                                        <dt>Size: </dt>
                                        <dd><span class="badge purple mr-1">{{product.DepartmentName}}</span></dd>
                                      </dl>
                                      <dl class="param param-inline small">
                                        <dt>Color: </dt>
                                        <dd> <span class="badge blue mr-1">{{product.CategoryName}}</span></dd>
                                      </dl>
                                </div>
                                <div class="col-12 col-sm-12 text-sm-center col-md-4 text-md-right row">
                                    <div class="col-3 col-sm-3 col-md-6 text-md-right" style="padding-top: 5px">
                                            <div class="price-wrap"> 
                                                    <var class="price">${{(product.Price * product.Quantity).toFixed(2)}}</var> 
                                                    <small class="text-muted">(${{product.Price.toFixed(2)}} each)</small> 
                                                </div>
                                    </div>
                                    <div class="col-4 col-sm-4 col-md-4">
                                        <div class="quantity">
                                            <input type="button" value="+" class="plus" (click)="onUpdateQuantity(1,  product.ProductId)">
                                            <input type="number" step="1" max="99" min="1" [value]="product.Quantity" title="Qty" class="qty"
                                                   size="4">
                                            <input type="button" value="-" class="minus" [disabled]="product.Quantity == 1" (click)="onUpdateQuantity(0, product.ProductId)">
                                        </div>
                                    </div>
                                    <div class="col-2 col-sm-2 col-md-2 text-right">
                                        <button type="button" class="btn btn-outline-danger btn-xs" (click)="onRemoveProductsFromCart(product.ProductId)">
                                            <i class="fa fa-trash" aria-hidden="true"></i>
                                        </button>
                                    </div>
                                </div>
                            </div>
                            <!-- END PRODUCT -->
                    </div>
                    <div class="card-footer">
                        <!-- <div class="coupon col-md-5 col-sm-5 no-padding-left float-left">
                            <div class="row">
                                <div class="col-6">
                                    <input type="text" class="form-control" placeholder="cupone code">
                                </div>
                                <div class="col-6">
                                    <input type="submit" class="btn btn-default" value="Use cupone">
                                </div>
                            </div>
                        </div> -->
                        <div class="float-right" style="margin: 10px">
                            <div class="float-left" style="margin: 17px">
                                Total price: <b>${{total.toFixed(2)}}</b>
                            </div>
                            <a class="btn btn-success" [routerLink]="['/checkout/review']" role="button">Checkout</a>
                            <!-- <app-paypal-checkout [totalAmount]="total.toFixed(2)" class="float-right"></app-paypal-checkout> -->
                        </div>
                    </div>
                </div>
            </div>
          </div>
          <div class="row" *ngIf="cart.length == 0">
            <div class="col-lg-12 p-5 bg-white rounded shadow-sm mb-5">
                <div class="jumbotron text-xs-center">
                    <h1 class="display-3">Oops!</h1>
                    <p class="lead">No Products Found in Cart</p>
                    <hr>
                    <p>
                        Please, Add Products to Cart
                    </p>
                    <p class="lead">
                        <a class="btn btn-success " [routerLink]="['/']" role="button">Our Products</a>
                    </p>
                </div>
            </div>
          </div>
    
          <!-- <div class="row pb-5 px-4 bg-white rounded shadow-sm">
            <div class="col-lg-6">
               <div class="bg-light rounded-pill px-4 py-3 text-uppercase font-weight-bold">Coupon code</div>
              <div class="p-4">
                <p class="font-italic mb-4">If you have a coupon code, please enter it in the box below</p>
                <div class="input-group mb-4 border rounded-pill p-2">
                  <input type="text" placeholder="Apply coupon" aria-describedby="button-addon3" class="form-control border-0">
                  <div class="input-group-append border-0">
                    <button id="button-addon3" type="button" class="btn btn-dark px-4 rounded-pill"><i class="fa fa-gift mr-2"></i>Apply coupon</button>
                  </div>
                </div>
              </div> 
              <div class="bg-light rounded-pill px-4 py-3 text-uppercase font-weight-bold">Instructions for seller</div>
              <div class="p-4">
                <p class="font-italic mb-4">If you have some information for the seller you can leave them in the box below</p>
                <textarea name="" cols="30" rows="2" class="form-control"></textarea>
              </div>
            </div>
            <div class="col-lg-6">
              <div class="bg-light rounded-pill px-4 py-3 text-uppercase font-weight-bold">Order summary </div>
              <div class="p-4">
                <p class="font-italic mb-4">Shipping and additional costs are calculated based on values you have entered.</p>
                <ul class="list-unstyled mb-4">
                  <li class="d-flex justify-content-between py-3 border-bottom"><strong class="text-muted">Order Subtotal </strong><strong>$390.00</strong></li>
                  <li class="d-flex justify-content-between py-3 border-bottom"><strong class="text-muted">Shipping and handling</strong><strong>$10.00</strong></li>
                  <li class="d-flex justify-content-between py-3 border-bottom"><strong class="text-muted">Tax</strong><strong>$0.00</strong></li>
                  <li class="d-flex justify-content-between py-3 border-bottom"><strong class="text-muted">Total</strong>
                    <h5 class="font-weight-bold">$400.00</h5>
                  </li>
                </ul><a href="#" class="btn btn-dark rounded-pill py-2 btn-block">Procceed to checkout</a>
              </div>
            </div>
          </div>
     -->
        </div>
      </div>
    <!-- End -->
  
  </div> 





## File: `frontend/src/index.html`

``` html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Frontend</title>
  <base href="/">

  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="icon" type="image/x-icon" href="favicon.ico">
  <link href="//cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/4.3.1/css/bootstrap.min.css" rel="stylesheet" />
</head>
<body>
  <app-root></app-root>
</body>
<!-- <script src="/node_modules/bootstrap/js/src/toast.js"></script> -->
</html>
 





