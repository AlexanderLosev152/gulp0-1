const { src, dest } = require("gulp");

const scss = require("gulp-sass")(require("sass"));
const concat = require("gulp-concat"); // concat - для обьединения файлов
const uglify = require("gulp-uglify-es").default; // для min.js

function scripts() {
  return src("src/js/main.js")
    .pipe(concat("main.min.js")) // перименовываем
    .pipe(uglify()) // минимизируем
    .pipe(dest("dist/js")); // выкинь все в папку js
}

function styles() {
  return src("src/scss/style.scss")
    .pipe(concat("style.min.css")) // обьединяем файлы css и перименовываем
    .pipe(scss({ outputStyle: "compressed" })) // для min css
    .pipe(dest("dist/css")); // для создания папки dist и в ней создать css
}

exports.styles = styles;
exports.scripts = scripts;
