<nav>
  <ul class="navigation">
    <li><a href="#">About us</a></li>
    <li><a href="#">Pricing</a></li>
    <li><a href="#">Contact</a></li>
  </ul>
  <div class="buttons">
    <a class="btn-main" href="#">Sign up</a>
    <a class="btn-hot" href="#">Get a quote</a>
  </div>
</nav>

- {
  margin: 0;
  padding: 0;
  }

$color-primary: #f9ed69;
$color-secondary: #f08a5d;
$color-tertiary: #b83b5e;
$color-text-dark: #333;
$width-button: 120px;

nav {
margin: 30px;
background-color: $color-primary;

&::after {
content: "";
clear: both;
display: table;
}

    }

.navigation {
list-style: none;

li {
display: inline-block;
margin-left: 30px;

    &:first-child {
      margin: 0;
    }

    a {
      text-decoration: none;
      text-transform: uppercase;
      color: $color-text-dark;
    }

}
}

    .buttons {
      float: right
    }

.btn-main:link,
.btn-hot:link {
padding: 10px;
display: inline-block;
text-align: center;
border-radius: 100px;
text-decoration: none;
text-transform: uppercase;
width: $width-button;
}
