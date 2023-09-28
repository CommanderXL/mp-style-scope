# style-scope

## Project setup

```javascript
// development
npm run serve // 小程序本地开发构建
```

代码编译后会同时输出微信和支付宝2个平台，最终的产出物在 `dist` 目录当中

### 微信和支付宝样式隔离场景：

**在 mpx 做基于微信跨支付宝小程序的开发过程中，为了和微信小程序默认的样式隔离特性保持一致，我们尝试将支付宝所有的页面/组件设置 `styleIsolation: "apply-shared"`。以下的异同点也是基于这个前提去梳理出来的**

不过在解除样式隔离的情况下遇到如下几种2个平台不太能保持表现一致的场景：

1. Page 和 Component 之间的样式影响（demo 当中的示例1）

场景一：

在微信环境下，只需要设置 component 的 `styleIsolation: "apply-shared"`，页面的配置不需要做任何改动，页面的样式规则即可直接影响这个 component。

在支付宝环境下，因为页面和组件都默认为 `styleIsolation: "apply-shared"`，那么如果想要页面的样式规则可以影响到 component，那么改动的方式是设置页面的 `styleIsolation: "shared"` （或者删除这个配置）。

此外在这个场景当中，微信和支付宝的改动对于其他组件的影响范围也不太能一致：

微信环境页面的样式只会影响到当前这个改动了 `styleIsolation: "apply-shared"` 配置的组件，而不会影响到其他默认走样式隔离的组件。

在支付宝环境下，因为页面的配置改为了 `styleIsolation: "shared"`，这也意味着页面的样式规则会影响到这个页面所引用的所有的组件。

场景二：

微信自定义组件的 `styleIsolation: "shared"` 配置也可以让页面样式规则影响到组件，而在支付宝的场景如果将自定义组件的配置改为 `shared`，页面的样式规则也不会影响到组件自身，除非将页面的配置改为 `shared`。


2. Component 和 Component 之间的样式影响（demo 当中的示例2）

微信和支付宝目前在自定义组件之间的样式解除策略基本一致：`shared` 去影响 `apply-shared` 或者 `shared`。

不过在影响范围这块不太能保持一致：在微信环境下，配置了 `styleIsolation: "shared"` 的组件只会影响到 `shared` 或者 `apply-shared` 组件，但是不会影响其他走默认样式隔离的组件。在支付宝环境只要设置了 `styleIsolation: "shared"` 的组件都会影响到其他的组件样式。



![style-scope](https://dpubstatic.udache.com/static/dpubimg/9LHreHGH8tUrrS_8cTe9G_style-scope-1.png)
