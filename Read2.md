## File: `index.html`

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Static Website</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <header>
        <h1>Welcome to My Static Website</h1>
        <nav>
            <ul>
                <li><a href="#about">About</a></li>
                <li><a href="#services">Services</a></li>
                <li><a href="#contact">Contact</a></li>
            </ul>
        </nav>
    </header>

    <section id="about">
        <h2>About Us</h2>
        <p>This is a basic static website for testing purposes.</p>
    </section>

    <section id="services">
        <h2>Our Services</h2>
        <p>We offer amazing services that are out of this world.</p>
    </section>

    <section id="contact">
        <h2>Contact Us</h2>
        <p>Feel free to reach out for more information!</p>
    </section>

    <footer>
        <p>&copy; 2024 My Static Website</p>
    </footer>
</body>
</html>

```




## File: `styles.css`

``` css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: Arial, sans-serif;
    line-height: 1.6;
    background-color: #f4f4f4;
    color: #333;
}

header {
    background: #333;
    color: #fff;
    padding: 1rem;
    text-align: center;
}

header h1 {
    margin-bottom: 1rem;
}

nav ul {
    list-style: none;
    padding: 0;
}

nav ul li {
    display: inline;
    margin: 0 1rem;
}

nav ul li a {
    color: #fff;
    text-decoration: none;
}

section {
    padding: 2rem;
    margin: 1rem 0;
}

footer {
    background: #333;
    color: #fff;
    text-align: center;
    padding: 1rem;
    margin-top: 2rem;
}

```




