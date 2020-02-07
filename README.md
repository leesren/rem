# px to rem

// rem.js

```js
function remResize(t) {
  var defaultSetting = {
    designWidth: 750,// 设计图宽度
    px2rem: 100,// 转换比例 1rem = 100px
    defaultFontSize: 20,// .5rem = 20px
    maxWidth: 0,
    dpr: parseInt(String(window.devicePixelRatio || 1))
  };
  var e = defaultSetting;
  var a = document.querySelector('meta[name="rem"]');
  if (a) {
    var i = a.getAttribute("content");
    if (i) {
      var n = i.match(/initial\-dpr=([\d\.]+)/);
      n && (e.dpr = parseFloat(n[1]));
      var o = i.match(/design\-width=([\d\.]+)/);
      o && (e.designWidth = parseFloat(o[1]));
      var r = i.match(/max\-width=([\d\.]+)/);
      r && (e.maxWidth = parseFloat(r[1]));
    }
  }
  // 设置html
  document.documentElement.setAttribute("data-dpr", e.dpr + ""),
    document.documentElement.setAttribute("max-width", e.maxWidth + "");
  // 设置viewport
  var s = document.querySelector('meta[name="viewport"]'),
    C = 1 / e.dpr,
    c =
      "width=device-width, initial-scale=" +
      C +
      ", minimum-scale=" +
      C +
      ", maximum-scale=" +
      C +
      ", user-scalable=no,viewport-fit=cover";
  s
    ? s.setAttribute("content", c)
    : ((s = document.createElement("meta")),
      s.setAttribute("name", "viewport"),
      s.setAttribute("content", c),
      document.head.appendChild(s));

  (e.get_px = function(t) {
    var a = parseInt(t);
    return !!a && e.dpr * a + "px";
  }),
    (e.resize = function() {
      var t = 0;
      try {
        t = document.documentElement.getBoundingClientRect().width;
      } catch (D) {}
      if ((t || (t = window.innerWidth), !t)) return !1;
      if (e.maxWidth) {
        var a = e.maxWidth * e.dpr;
        t > a && (t = a);
      }
      var i = (t / (e.designWidth / e.px2rem) / e.defaultFontSize) * 100,
        n = "rem",
        o = document.getElementById(n);
      o
        ? (o.innerHTML = "html{font-size:" + i + "% !important;}")
        : ((o = document.createElement("style")),
          o.setAttribute("id", n),
          (o.innerHTML = "html{font-size:" + i + "% !important;}"),
          document.head.appendChild(o)),
        e.callback && e.callback();
    }),
    e.resize(),
    window.addEventListener(
      "resize",
      function() {
        clearTimeout(e.tid), (e.tid = setTimeout(e.resize, 33));
      },
      !1
    ),
    window.addEventListener("load", e.resize, !1);

  var B = (document.documentElement.clientWidth / 375) * 100;
  return (
    (document.getElementsByTagName("html")[0].style.fontSize = B + "px"),
    document.getElementsByTagName("html")[0].classList.add("fontinit"),
    t && t(),
    B
  );
}
remResize()

```
