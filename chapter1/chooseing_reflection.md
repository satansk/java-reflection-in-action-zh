# 1.2.1 选择反射

给定一个组件，代码必须实现两步：

1. 找到该组件支持的 `setColor` 方法。
2. 使用合适的 `Color` 调用 `setColor` 方法。

有几种方式可以完成上面两个步骤，下面查看每个方法的优缺点。

1. 如果 George 的团队可以控制所有代码，可以重构组件代码，使其实现一个公共接口，该接口中声明 `setColor` 方法，之后每个组件都可以使用该接口引用，可以用接口调用 `setColor` 方法，无需知道组件具体类型。但是团队无法控制标准 Java 组件以及第三方组件。

2. 还可以为每个组件实现一个适配器（adapter），每个适配器可以实现一个公共接口，并将 `setColor` 方法委托给具体组件。然而，团队使用了大量的组件类，该方案将会造成类数量爆炸，进而导致运行时系统中实例数量爆炸。

3. 使用 `instanceof` 和 `cast` 在运行时识别具体类型，该方案会导致维护问题。首先，随着条件语句和类型转换语句的添加，代码数量会膨胀，其次，代码会与具体类型耦合在一起，进而导致添加、修改、删除组件很困难。

上面 3 中方案都涉及到修改代码适配组件，或者修改代码发现组件类型。George 认识到只要找到 `setColor` 方法并且调用它即可，在学习了一点反射知识后，George 知道了如何在运行时查询方法的调用对象的类型，以及如何使用反射进行方法调用。反射很适合解决该问题，因为它没有使用类型信息过于限制解决方案。