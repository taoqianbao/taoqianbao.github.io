---
title: 前端代码打包工具之gulp
tags: [package, gulp, javascript]
p: javascript/cli-gulp-guide
date: 2014-01-23 09:36:58
categories: Javascript
---

## 前言

<!--more-->

## 正文

### 老WEB项目(JSP,PHP)中想使用less等打包工具方法 

``` JS
var gulp = require('gulp');
var less = require('gulp-less');
var path = require('path');
var minify = require('gulp-minify-css');

var paths = {
    'less': ['../css/less/*.less'],
    'css': '../css/',
    'css-release': '../css/release/'
};

gulp.task('less', function () {
    gulp.src(paths.less)
        .pipe(less())
        .pipe(gulp.dest(paths.css))
        .pipe(minify())
        .pipe(gulp.dest(paths['css-release']));
});

gulp.task('watch', function () {
    gulp.watch(paths.less, ['less']);
});

gulp.task('default', ['less']);
```


### 基于gulp的H5-rem2px项目方案

#### 项目架构
``` JS
- package.json
- gulpfile.js
- src/asset/css/***.less
```

package.json

``` JS
{
  "name": "less-gulp-cli",
  "version": "1.0.0",
  "description": "",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "lint": "gulp lint",
    "watch": "gulp watch",
    "build": "cross-env NODE_ENV=production gulp build"
  },
  "author": "peter",
  "license": "ISC",
  "dependencies": {},
  "devDependencies": {
    "babel-preset-es2015": "^6.18.0",
    "cross-env": "^3.1.3",
    "eslint": "^3.8.1",
    "eslint-config-standard": "^6.2.1",
    "eslint-friendly-formatter": "^2.0.6",
    "eslint-plugin-json": "^1.2.0",
    "eslint-plugin-promise": "^3.3.0",
    "eslint-plugin-standard": "^2.0.1",
    "generate-weapp-page": "^0.1.3",

    "autoprefixer": "^7.2.5",
    "cssnext": "^1.8.4",
    "del": "^3.0.0",
    "gulp": "^3.9.1",
    "gulp-babel": "^6.1.2",
    "gulp-cssnano": "^2.1.2",
    "gulp-eslint": "^3.0.1",
    "gulp-htmlmin": "^3.0.0",
    "gulp-if": "^2.0.1",
    "gulp-imagemin": "^3.0.3",
    "gulp-jsonminify": "^1.0.0",
    "gulp-less": "^3.1.0",
    "gulp-load-plugins": "^1.5.0",
    "gulp-postcss": "^7.0.1",
    "gulp-rename": "^1.2.2",
    "gulp-sourcemaps": "^2.2.0",
    "gulp-uglify": "^2.0.0",
    "gulp-util": "^3.0.7",
    "inquirer": "^1.2.2",
    "postcss": "^6.0.16",
    "postcss-px2rem": "^0.3.0",
    "postcss-pxtorem": "^4.0.1",
    "precss": "^3.0.0",
    "run-sequence": "^2.2.1"
  }
}

```

gulpfile.js

``` JS

/*
  使用Flexible实现手淘H5页面的终端适配
  自动化gulp编译工具
  可以通过任意开发工具完成`src`下的编码，`gulp`会监视项目根目录下`src`文件夹，当文件变化自动编译
  
  环境：
  1、安装node
  2、安装gulp
  3、启动项目：npm run watch
  4、最终编写的文件会保存在dist目录
*/

var gulp = require('gulp');
const gulpLoadPlugins = require('gulp-load-plugins')
var postcss = require('gulp-postcss');
const del = require('del')
const runSequence = require('run-sequence')

var autoprefixer = require('autoprefixer');
var cssnext = require('cssnext');
var precss = require('precss');
var px2rem = require('postcss-px2rem'); 
// https://www.npmjs.com/package/px2rem

// load all gulp plugins
const plugins = gulpLoadPlugins()
const env = process.env.NODE_ENV || 'development'
const isProduction = () => env === 'production'

// processors规则
var processors = [
  autoprefixer, // 处理浏览器私有前缀
  cssnext,      // 使用CSS未来的语法
  precss,       // 像Sass的函数
  px2rem({
    remUnit: 75, // set `rem` unit value (default: 75)
    // baseDpr: 2    // set base device pixel ratio (default: 2)
  })
];

/**
 * Clean distribution directory
 */
gulp.task('clean', del.bind(null, ['dist/*']))

/**
 * Lint source code
 */
gulp.task('lint', () => {
  // return gulp.src(['*.{js,json}', '**/*.{js,json}', '!node_modules/**', '!dist/**', '!**/bluebird.js'])
  //   .pipe(plugins.eslint())
  //   .pipe(plugins.eslint.format('node_modules/eslint-friendly-formatter'))
  //   .pipe(plugins.eslint.failAfterError())
})

/**
 * Compile js source to distribution directory
 */
gulp.task('compile:js', () => {
  return gulp.src(['src/**/*.js'])
    .pipe(plugins.babel())
    .pipe(plugins.if(isProduction, plugins.uglify()))
    .pipe(gulp.dest('dist'))
})

/**
 * Compile css source to distribution directory
 */
gulp.task('compile:css', function () { 
  
  return gulp.src('src/**/*.css')
    .pipe(postcss(processors))
    .pipe(gulp.dest('dist')); 
});

/**
 * Compile less source to distribution directory
 */
gulp.task('compile:less', () => {
  return gulp.src(['src/**/*.less'])
    .pipe(plugins.less())
    .pipe(postcss(processors))
    .pipe(plugins.if(isProduction, plugins.cssnano({ compatibility: '*' })))
    .pipe(gulp.dest('dist'))
})

/**
 * Compile json source to distribution directory
 */
gulp.task('compile:json', () => {
  return gulp.src(['src/**/*.json'])
    .pipe(plugins.jsonminify())
    .pipe(gulp.dest('dist'))
})


/**
 * Compile img source to distribution directory
 */
gulp.task('compile:img', () => {
  return gulp.src(['src/**/*.{jpg,jpeg,png,gif}'])
    .pipe(plugins.imagemin())
    .pipe(gulp.dest('dist'))
})

/**
 * Compile html source to distribution directory
 */
gulp.task('compile:html', () => {
  return gulp.src(['src/**/*.html'])
    .pipe(gulp.dest('dist'))
})

/**
 * Compile source to distribution directory
 */
gulp.task('compile', ['clean'], next => {
  runSequence([
    'compile:js',
    'compile:css',
    'compile:less',
    'compile:json',
    'compile:img',
    'compile:html'
  ], next)
})

/**
 * Copy extras to distribution directory
 */
gulp.task('extras', [], () => {
  return gulp.src([
    'src/**/*.*',
    '!src/**/*.js',
    '!src/**/*.css',
    '!src/**/*.less',
    '!src/**/*.json',
    '!src/**/*.{jpe?g,png,gif}',
    '!src/**/*.html'
  ])
  .pipe(gulp.dest('dist'))
})

/**
 * Build
 */
gulp.task('build', ['lint'], next => runSequence(['compile', 'extras'], next))


/**
 * Watch source change
 */
gulp.task('watch', ['build'], () => {
  gulp.watch('src/**/*.js', ['compile:js'])
  gulp.watch('src/**/*.css', ['compile:css'])
  gulp.watch('src/**/*.less', ['compile:less'])
  gulp.watch('src/**/*.json', ['compile:json'])
  gulp.watch('src/**/*.{jpe?g,png,gif}', ['compile:img'])
  gulp.watch('src/**/*.html', ['compile:html'])
})

/**
 * Default task
 */
gulp.task('default', ['watch'])

```

src/asset/css/***.less
``` LESS

/*函数表达式*/
.wrap () {
    text-wrap: wrap;
    white-space: pre-wrap;
    white-space: -moz-pre-wrap;
    word-wrap: break-word;
}

pre { 
    .wrap; 
    div {
        width: 100px;
    }
}


/*mixin*/
.mixin (dark, @color) {
    color: darken(@color, 10%);
}
.mixin (light, @color) {
    color: lighten(@color, 20%);
}
.mixin (@_, @color) {
    display: block;
}

@switch: dark;

.class {
  .mixin(@switch, #888);
}



/*参数形式*/
.border-radius (@radius) {
    border-radius: @radius;
    -moz-border-radius: @radius;
    -webkit-border-radius: @radius;
}

#header {
    .border-radius(4px);
}

.button {
    .border-radius(6px);  
}


/*嵌套形式*/
#header {
    color: black;
    .navigation {
        font-size: 12px;
    }
    .logo {
        width: 300px;
        &:hover { text-decoration: none }
    }
}

.bordered {
    &.float {
        float: left; 
    }
    .top {
        margin: 5px; 
    }
}
```


## 小结

本文示例代码：[打包工具示例](https://github.com/taoqianbao/tools-cli-guide)

可以参考以下资料：
https://github.com/amfe/article/issues/17?utm_source=caibaojian.com
https://www.w3cplus.com/PostCSS/postcss-quickstart-guide-gulp-setup.html
  

## 关于作者
** 珠峰
WEB开发与管理相结合，注重技术与应用结合。现居上海。 
