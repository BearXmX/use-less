# use-less
less的一些用法

## `@ => 定义变量`

```less
@ellipsis: 1,2,3,4,5;

@colors: #00b96b,#40a9ff;

@record: {
  @width: 1200px;
  @color: #000;
};

// 使用
body {
  color: @record[@color];
}

// 编译
body {
  color: #000; 
}
```

## `extract函数`

```less
@colors: #00b96b,#40a9ff;

// 使用
body {
  color: extract(@colors, 2);
}

// 编译
body {
  color: #40a9ff; 
}
```

## `range函数`

```less
range(8) // => 1,2,...,8;

@ellipsis: range(8); // => @ellipsis: 1,2,...,8;
```

## `each函数`

```less
@ellipsis: range(8);

// 使用
each(@ellipsis, {
   // @value、@index、@key 定义类名上用{}包裹
  .ellipsis-@{value} {
    display: -webkit-box;
    -webkit-box-orient: vertical;
    text-overflow: ellipsis;
    overflow: hidden;
    -webkit-line-clamp:@value;
  }
});

// 编译
 .ellipsis-1 {
  display: -webkit-box;
  -webkit-box-orient: vertical;
  text-overflow: ellipsis;
  overflow: hidden;
  -webkit-line-clamp: 1;
}

.ellipsis-2 {
  display: -webkit-box;
  -webkit-box-orient: vertical;
  text-overflow: ellipsis;
  overflow: hidden;
  -webkit-line-clamp: 2;
} 
```

## `混入`

`普通混入`
```less
.body-color {
  color: #000;
}

body {
  .body-color();
}

// 编译
body{
  color: #000;
}
```

`传参混入`
```less
// 可设置默认值
.body-color(@color: #00b96b) {
  color: @color;
}

body {
  .body-color(#fff;);
}

// 编译
body{
  color: #fff;
}
```

`进阶混入`

```less
.body-color(@color,@styles) {
  color: @color;
  @styles();
}  

body {
  .body-color(#fff,{
     font-size: 18px;
  });
}

// 编译
body{
  color: #fff;
  font-size: 18px;
}
```

## `进阶案例`

`不同分辨率处理`
```less
@union: {
  @0: {
    @width: 1200px;
    @style: {
      color: #000;
    };
  };
  @1: {
    @width: 800px;
    @style: {
      color: #00b96b;
    };
  };
  @2: {
    @width: 400px;
    @style: {
      color: #40a9ff;
    };
  };
};

.body-media(@style,@width) {
  @media screen and (max-width: @width) {
    @style();
  }
}

body {
  each(@union, {
    @record: @value;
    .body-media(@record[@style],@record[@width])
  });
}

// 编译
@media screen and (max-width: 1200px) {
  body {
    color: #000;
  }
}
@media screen and (max-width: 800px) {
  body {
    color: #00b96b;
  }
}
@media screen and (max-width: 400px) {
  body {
    color: #40a9ff;
  }
}
```
