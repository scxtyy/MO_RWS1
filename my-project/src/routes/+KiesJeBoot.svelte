
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Rijkswaterstaat Experience</title>
  <style>
    header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 1rem;
      background-color: #f1f1f1;
    }

    .logo {
      display: flex;
      align-items: center;
    }

    .logo img {
      height: 40px;
      margin-right: 1rem;
    }

    .nav-links {
      display: flex;
      align-items: center;
    }

    .nav-links a,
    .contact-button {
      margin-left: 1rem;
      text-decoration: none;
      color: #333;
      background: none;
      border: none;
      cursor: pointer;
      position: relative;
    }

    .phone-number {
      display: none;
      position: absolute;
      background-color: yellow;
      color: black;
      padding: 0.5rem;
      border-radius: 5px;
      top: 100%;
      left: 0;
      z-index: 10;
      white-space: nowrap;
    }

    .contact-button:hover + .phone-number {
      display: block;
    }

    main {
      text-align: center;
      padding: 2rem;
      background: linear-gradient(to bottom, #d9edf7, #fff);
    }

    .cards {
      display: flex;
      justify-content: space-around;
      margin-top: 2rem;
    }

    .card {
      background-color: #0066cc;
      border-radius: 8px;
      color: white;
      padding: 1rem;
      width: 200px;
      text-align: center;
    }

    .card img {
      width: 100%;
      border-bottom: 2px solid white;
      margin-bottom: 1rem;
    }

    .card .attributes {
      margin: 1rem 0;
    }

    .card .attributes p {
      margin: 0.5rem 0;
    }

    .card .attributes span {
      display: inline-block;
      width: 15px;
      height: 15px;
      background-color: white;
      margin-right: 2px;
    }

    .card .attributes span.filled {
      background-color: yellow;
    }

    .card button {
      background-color: yellow;
      border: none;
      padding: 0.5rem 1rem;
      cursor: pointer;
    }

    footer {
      background-color: #005580;
      color: white;
      padding: 1rem;
      text-align: center;
    }
  </style>
</head>
<body>
  <header>
    <div class="logo">
      <img src="/rijkswaterLogo.png" alt="Rijkswaterstaat Logo">
      <h1>Rijkswaterstaat experience</h1>
    </div>
    <div class="nav-links">
      <!-- svelte-ignore a11y-invalid-attribute -->
      <a href="#">International</a>
      <button class="contact-button">Contact</button>
      <span class="phone-number">123-456-7890</span>
    </div>
  </header>
  <main>
    <h2>Kies je boot</h2>
    <div class="cards">
      <div class="card">
        <img src="/Boot1RWS.png" alt="Boat 1">
        <div class="attributes">
          <p>Snelheid</p>
          <div>
            <span class="filled"></span><span class="filled"></span><span class="filled"></span><span></span><span></span>
          </div>
          <p>Diepte</p>
          <div>
            <span class="filled"></span><span class="filled"></span><span></span><span></span><span></span>
          </div>
        </div>
        <button>Volgende</button>
      </div>
      <div class="card">
        <img src="/Boot2RWS.png" alt="Boat 2">
        <div class="attributes">
          <p>Snelheid</p>
          <div>
            <span class="filled"></span><span class="filled"></span><span></span><span></span><span></span>
          </div>
          <p>Diepte</p>
          <div>
            <span class="filled"></span><span class="filled"></span><span class="filled"></span><span class="filled"></span><span></span>
          </div>
        </div>
        <button>Volgende</button>
      </div>
      <div class="card">
        <img src="/Boot3RWS.png" alt="Boat 3">
        <div class="attributes">
          <p>Snelheid</p>
          <div>
            <span class="filled"></span><span class="filled"></span><span class="filled"></span><span class="filled"></span><span></span>
          </div>
          <p>Diepte</p>
          <div>
            <span class="filled"></span><span class="filled"></span><span></span><span></span><span></span>
          </div>
        </div>
        <button>Volgende</button>
      </div>
    </div>
  </main>
  <footer>
    <p>Minor opdracht Rijkswaterstaat</p>
  </footer>
</body>
</html>
