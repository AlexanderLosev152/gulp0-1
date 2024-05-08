const { src, dest, watch, parallel, series } = require("gulp");

const scss = require("gulp-sass")(require("sass"));
const concat = require("gulp-concat"); // concat - для обьединения файлов
const uglify = require("gulp-uglify-es").default; // для min.js
const browserSync = require("browser-sync").create(); // для перезагрузки страницы при изменениях
const clean = require("gulp-clean"); // удалять папку build при каждой перезаписи

function scripts() {
  return src([
    "node_modules/swiper/swiper-bundle.js", // подключение swiper
    "src/js/main.js",
  ])
    .pipe(concat("main.min.js")) // перименовываем
    .pipe(uglify()) // минимизируем js
    .pipe(dest("build/js")) // выкинь все в папку js
    .pipe(browserSync.stream()); // слежка за изменениями
}

function styles() {
  return src("src/scss/style.scss")
    .pipe(concat("style.min.css")) // обьединяем файлы css и перименовываем
    .pipe(scss({ outputStyle: "compressed" })) // минимизируем css
    .pipe(dest("build/css")) // выкинь все в папку css
    .pipe(browserSync.stream()); // слежка за изменениями
}

function watching() {
  browserSync.init({
    server: {
      baseDir: "src/",
    },
  });
  watch(["src/scss/style.scss"], styles); // следить за изменениями в стилях
  watch(["src/js/main.js"], scripts); // следить за изменениями в скриптах
  watch(["src/**/*.html"]).on("change", browserSync.reload); // следить за изменениями в html и перезагружать страницу
}

function cleanDist() {
  return src().pipe(clean());
}

function building() {
  return src(["src/css/style.min.css", "src/js/main.min.js", "src/**/*.html"], {
    base: "src",
  }).pipe(dest("build"));
}

exports.styles = styles;
exports.scripts = scripts;
exports.watching = watching;

exports.build = series(cleanDist, building);
exports.default = parallel(styles, scripts, watching);
