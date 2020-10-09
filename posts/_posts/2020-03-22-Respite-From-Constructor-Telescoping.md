---
layout: post
title: Respite From Constructor Telescoping
tags: Programming, Java, Design Patterns
cover_url: https://github.com/alivcor/lightforest/raw/master/arch.jpg
cover_meta: 
  Wall Art (c) AD Photography
color_scheme: tango
mathjax: true
mathjax: True
---
<style TYPE="text/css">
code.has-jax {font: inherit; font-size: 100%; background: inherit; border: inherit;}
</style>
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
    tex2jax: {
        inlineMath: [['$','$']],
        skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'] // removed 'code' entry
    }
});
MathJax.Hub.Queue(function() {
    var all = MathJax.Hub.getAllJax(), i;
    for(i = 0; i < all.length; i += 1) {
        all[i].SourceElement().parentNode.className += ' has-jax';
    }
});
</script>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.4/MathJax.js?config=TeX-AMS_HTML-full"></script>


## Telescoping

Lets have a look at the constructor below


```java
public class Car {
  int modelYear;
  String modelName;
  String color;
  String manufacturer;
  double sellingPrice;
  String carType;
  Boolean isLuxury;
  Boolean isImported;
  String countryOfManufacture;

  public Car(int modelYear) {
    this.modelYear = modelYear;
  }

  public Car(int modelYear, String modelName) {
    this(modelYear);
    this.modelName = modelName;
  }

  public Car(int modelYear, String modelName, String color) {
    this(modelYear, modelName);
    this.color = color;
  }

  public Car(int modelYear, String modelName, String color, String manufacturer) {
    this(modelYear, modelName, color);
    this.manufacturer = manufacturer;
  }

...

  public Car(int modelYear, String modelName, String color, String manufacturer, double sellingPrice, String carType, Boolean isLuxury, Boolean isImported, String countryOfManufacture) {
    modelYear = modelYear;
    modelName = modelName;
    ...
  }
}
```

It just looks ugly. And if you were to look at this code, you could figure out it wasn't the work of an Amateur programmer. This is one of those things which make you wonder _why doesn't my code look clean and professional_ (Feel me?)

Believe it or not, there's a term for it - it's called *Constructor Telescoping*. When you load your constructor with a shit ton of parameters, it makes the code look ugly. I, myself have done it many times, and now that I know there's something I can do to avoid it - I am not going down this ugly lane anymore.

### Builder Pattern

If you ever used the `StringBuilder` in Java - that's an example of the Builder pattern. Today, we're going to create our very own Builder class! Hang Tight!

#### Java Beans Crash Course

Rules for a class to qualify as a "Java Bean" :

1. The class must have a public default constructor (with no arguments). 
2. Must have a shit ton of getters and setters
3. The class should be serializable. (implements `java.io.Serializable`)

So to make our `Car` class a Java Bean, we just have to change it to look like this:

```java
    public class Car {
        private int modelYear;
        private String modelName;
        private String color;
        private String manufacturer;
        private double sellingPrice;
        private String carType;
        private Boolean isLuxury;
        private Boolean isImported;
        private String countryOfManufacture;

        /** No-arg constructor (takes no arguments). */
        public Car() { }

        public getModelYear(){
            return modelYear;
        }

        public setModelYear(int year){
            modelYear = year;
        }

        ...
        ...


    }
```

There are two obvious problems here:

1. Lack of Immutability - Thread safety is a problem
2. _We The People_ worth of setters and getters and yet it's hard to enfore combination rules (eg; A `Car` must have a `manufacturer` and a `modelName`!

To get around that, we could resort to having multiple Telescoped Constructors (and miss out on having all permutations, and get an even uglier looking code), or, we could just learn the Builder Pattern (finally! )

### Builder Pattern

<script src="https://gist.github.com/alivcor/40fc44c1b28286481944e113b24de245.js"></script>
    

Now lets have a one mile overview of the code above, because right now - it looks pretty daunting.

![One Mile View](https://github.com/alivcor/lightforest/raw/master/allcode.png)

Alright, so what did we just achieve with making our code look scarier?

#### All Cars built will require a manufacturer and a model name

Observer how IDE shows a red mark below `Car.builder`

![IDE Code Check](https://github.com/alivcor/lightforest/raw/master/error_builder.png)


#### You can play with rest of the permutations cleanly, while keeping the immutability (and thus thread safety)


![Car Demo](https://github.com/alivcor/lightforest/raw/master/car_demo.png)




Feeling generous ? Help me write more blogs like this :)  

<script type="text/javascript" src="https://cdnjs.buymeacoffee.com/1.0.0/button.prod.min.js" data-name="bmc-button" data-slug="abhinandandubey" data-color="#FFDD00" data-emoji=""  data-font="Cookie" data-text="Buy me a coffee" data-outline-color="#000" data-font-color="#000" data-coffee-color="#fff" ></script>