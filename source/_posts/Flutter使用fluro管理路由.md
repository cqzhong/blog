---
title: Flutter使用fluro管理路由
urlname: smencz
date: 2021-08-20 17:00:00 +0800
tags: [flutter]
categories: [flutter]
---

### 1、pubspec.yaml 文件内导入

```
fluro: "^2.0.3"
```

<!-- more -->

### 2、lib 下新建 routers 文件夹, 及各个文件说明

```
.
└── lib
    └── routers
        ├── fluro_application.dart
        ├── fluro_routes.dart
        ├── router.dart
        └── router_handler.dart
- 新建fluro_application.dart文件,初始化fluro
- 新建route_handlers.dart文件,用于初始化跳转到各个页面的handle,并获取到上个页面传递过来的值,然后在初始化要跳转到的页面.
- 新建fluro_routes.dart文件,用于绑定路由地址和对应的handler.
- 新建 router.dart文件用于导入多个文件
```

- fluro_application.dart 文件

```dart
import 'package:fluro/fluro.dart';

enum ENV {
  PRODUCTION,
  DEV,
}

/// 全局对象
class FluroApplication {
  static FluroRouter? router;

  /// 通过Application设计环境变量
  static ENV env = ENV.DEV;
}
```

- fluro_routes.dart 文件

```dart
import 'package:flutter/material.dart';
import 'package:fluro/fluro.dart';
import 'dart:convert' as convert;

import '/pages/empty/empty_page.dart';
import './router_handler.dart';
import 'fluro_application.dart';

class FluroRoutes {
  // 路由管理
  // static FluroRouter fluroRouter;

  // 路由的命名使用 中划线 -
  static String root = '/';
  static String detailPage = "/detail";
  static String tagPage = "/tag";
  static String tagListPage = "/tag-list";

  static void configureRoutes(FluroRouter router) {
    // 未发现对应route
    router.notFoundHandler = new Handler(
        handlerFunc: (BuildContext? context, Map<String, List<String>> params) {
      print('404');
      return EmptyPage();
    });

    router.define(root, handler: rootHandler);
    router.define(detailPage, handler: detailHandler);
    router.define(tagPage, handler: tagHandler);
    router.define(tagListPage, handler: tagListHandler);
  }

  /// 对参数进行encode，解决参数中有特殊字符，影响fluro路由匹配
  static Future navigateTo(BuildContext context, String path,
      {Map<String, dynamic>? params,
      TransitionType transition = TransitionType.cupertino,
      bool replace = false,
      bool clearStack = false,
      bool maintainState = true,
      bool rootNavigator = false,
      Duration? transitionDuration,
      RouteTransitionsBuilder? transitionBuilder,
      RouteSettings? routeSettings}) {
    String query = '';
    if (params != null) {
      int index = 0;
      for (final key in params.keys) {
        final parameterEncode = convert.jsonEncode(params[key]);
        // print('-------$parameterEncode: ${parameterEncode.runtimeType}');
        // final parameterDncode = convert.jsonDecode(parameterEncode);
        // print('two----$parameterDncode: ${parameterDncode.runtimeType}');
        final value = Uri.encodeComponent(parameterEncode);
        if (index == 0) {
          query = '?';
        } else {
          query = query + '\&';
        }
        query += '$key=$value';
        index++;
      }
    }
    print('我是navigateTo传递的参数：$query');
    // routeSettings

    path = path + query;
    return FluroApplication.router!.navigateTo(
      context,
      path,
      transition: transition,
    );
  }
}

```

- router_handler.dart 文件

```dart
import 'package:fluro/fluro.dart';
import 'package:flutter/material.dart';
import 'dart:convert' as convert;

import '../pages/tabbar/tab_bar.dart';
import '/pages/detail/detail_page.dart';
import '/pages/tag/tag_list_page.dart';
import '/pages/tag/tag_page.dart';

/// 根页面
var rootHandler = Handler(
    handlerFunc: (BuildContext? context, Map<String, List<String>> params) {
  // FluroRouter.appRouter.printTree();
  return TabBarPage();
});

/// 博客内容详情页
Handler detailHandler = Handler(
    handlerFunc: (BuildContext? context, Map<String, List<String>> params) {
  String path = convert.jsonDecode(params['path']!.first);
  String name = convert.jsonDecode(params['name']!.first);
  return DetailPage(
    path: path,
    name: Uri.decodeComponent(name),
  );
});

/// 所有文章标签页面
Handler tagHandler = Handler(
    handlerFunc: (BuildContext? context, Map<String, List<String>> params) {
  return TagPage();
});

/// 某一标签下的所有文章列表页面
Handler tagListHandler = Handler(
    handlerFunc: (BuildContext? context, Map<String, List<String>> params) {
  String name = convert.jsonDecode(params['name']!.first);
  bool isTag = convert.jsonDecode(params['isTag']?.first ?? 'true');
  // print('name: $name  isTag:$isTag ${isTag.runtimeType}');
  return TagListPage(name: name, isTag: isTag);
});
```

- router.dart 文件

```dart
library routers;

export 'package:fluro/fluro.dart';

export './fluro_application.dart';
export './fluro_routes.dart';
```

### 3、main.dart 文件内引入

```dart
import 'routers/router.dart';

void main() {
  final router = FluroRouter();
  FluroRoutes.configureRoutes(router);
  FluroApplication.router = router;
  runApp(
    MaterialApp(
      title: "cqzhong's blog",
      // debugShowCheckedModeBanner: true, // 隐藏测试
      onGenerateRoute: FluroRouter.appRouter.generator,
      theme: ThemeData(
          // platform: TargetPlatform.iOS,
          primaryColor: Colors.white,
          backgroundColor: ColorConstant.PRIMARY_BACKGROUND),
      localizationsDelegates: [
        RefreshLocalizations.delegate,
        GlobalWidgetsLocalizations.delegate,
        GlobalMaterialLocalizations.delegate
      ],
      supportedLocales: [
        const Locale('en'),
        const Locale('zh'),
      ],
      localeResolutionCallback:
          (Locale? locale, Iterable<Locale> supportedLocales) {
        return locale;
      },
      home: TabBarPage(),
      builder: EasyLoading.init(),
    )
  );
}
```

### 4、无参跳转

```dart
FluroRoutes.navigateTo(navContext, FluroRoutes.tagPage);
```

### 5、有参跳转

```dart
FluroRoutes.navigateTo(context, FluroRoutes.tagListPage,
            params: {'name': model.name, 'isTag': false});
```

### 6、修改跳转动画

```dart
FluroRoutes.navigateTo(navContext, FluroRoutes.imagePreviewPage,
            transition: TransitionType.fadeIn);
```

### 7、接收页面回传参数

```dart
FluroRoutes.navigateTo(context, FluroRoutes.tagListPage,
                              params: {'name': tag})
                          .then((res) => {Utils.showToast('$res')});

 Navigator.of(context).pop({'key': 'v'})
```

### 8、跳转页面、清理导航栈

```dart
FluroRoutes.navigateTo(navContext, FluroRoutes.tagPage, clearStack: true);
```
