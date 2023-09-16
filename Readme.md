# Endless Scroll

## HTML

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Endless Scroll</title>
    <link rel="stylesheet" href="style.css" />
  </head>
  <body>
    <h1>ENDLESS SCROLL</h1>
    <div class="img-container" id="img-container"></div>
    <script defer src="index.js"></script>
  </body>
</html>

```

## CSS

```
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: "Sedgwick Ave Display", cursive;
  background-color: whitesmoke;
}

h1 {
  text-align: center;
  margin-top: 25px;
  margin-bottom: 45px;
  font-size: 40px;
  letter-spacing: 1px;

}

.img-container {
  margin: 10px 30%;
}

.img-container img {
  width: 100%;
  margin-top: 5px;
}


@media screen and (max-width: 600px) {
  h1 {
    font-size: 20px;
  }
  .img-container {
    margin: 10px;
  }
}


```

## JS

```
const imageContainer = document.getElementById("img-container");

let photosArray = [];

let ready = false;
let imagesLoaded = 0;
let totalImages = 0;

const count = 10;
const apikey = "alG7OhSYiSVHWPUrodbTbJetFhdju6NI0NYc8_d2ZUk";
const apiUrl = `https://api.unsplash.com/photos/random/?client_id=${apikey}&count=${count}`;

function setAttribute(element, attributes) {
  for (const key in attributes) {
    element.setAttribute(key, attributes[key]);
  }
}

function imageLoaded() {
  imagesLoaded++;
  if (imagesLoaded === totalImages) {
    ready = true;
  }
}

function displayPhotos() {
  totalImages = photosArray.length;

  imagesLoaded = 0;

  photosArray.forEach((photo) => {
    const item = document.createElement("a");

    setAttribute(item, {
      href: photo.links.html,
      target: "_blank",
    });

    const img = document.createElement("img");
    setAttribute(img, {
      src: photo.urls.regular,
      alt: photo.alt_description,
      title: photo.alt_description,
    });

    img.addEventListener("load", imageLoaded);

    item.append(img);

    imageContainer.append(item);
  });
}

async function getPhotos() {
  try {
    const response = await fetch(apiUrl);
    photosArray = await response.json();
    displayPhotos();
  } catch (error) {
    console.log(error);
  }
}

window.addEventListener("scroll", () => {
  if (
    window.scrollY + window.innerHeight >= document.body.offsetHeight &&
    ready
  ) {
    ready = false;
    getPhotos();
  }
});

getPhotos();


```
