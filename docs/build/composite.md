# Composite Pattern 组合模式

我们在解决平时开发中遇到的树形结构问题中，比如常遇到的组织机构树的设计，我们希望把所有组织节点都能简单使用但又保留它们之间的层级关系，这种将简单元素和复杂元素之间模糊处理的设计模式就是**组合模式**。

## 定义

**组合模式：** 将对象组合成树形结构以表示“部分-整体”的层次结构，组合模式使得用户对单个对象和组合对象的使用具有一致性。掌握组合模式的重点是要理解清楚 “部分/整体” 还有 ”单个对象“ 与 "组合对象" 的含义。

## 角色分析

![组合模式UML类图](../../static/composite.png)

从图1中可以看出，组合模式涉及到的角色有以下几种：

+ **Component 抽象节点：** 是组合中的对象声明接口，在适当的情况下，实现所有类共有接口的默认行为。声明一个接口用于访问和管理Component子部件。

+ **Leaf 叶子节点：** 在组合中表示叶子结点对象，叶子结点没有子结点。

+ **Composite 树枝节点：** 定义有枝节点行为，用来存储子部件，在Component接口中实现与子部件有关操作，如增加(add)和删除(remove)等。

## 示例

下面以组织机构树的设计作为栗子演示组合模式的应用：

### Component 抽象节点

```java
public abstract class Organization {

    /**
     * 抽象添加方法
     */
    public abstract void add(Organization org);

    /**
     * 抽象删除方法
     */
    public abstract void remove(Organization org);

    /**
     * 抽象展示方法
     */
    public abstract void dispaly(int index);
}
```

### Leaf 叶子节点

```java
public class LeafOrg extends Organization {

    @Override
    public void add(Organization org) {
        System.out.println("叶子组织节点没有添加实现");
    }

    @Override
    public void remove(Organization org) {
        System.out.println("叶子组织节点没有移除实现");
    }

    @Override
    public void dispaly(int index) {
        System.out.println("第" + index + "层组织机构");
    }
}

```

### Composite 树枝节点

```java
public class ChildOrg extends Organization{

    /**
     * 子节点组织机构
     */
    private List<Organization> children = new ArrayList<Organization>();

    /**
     * 组织节点名称
     */
    private String orgName;

    public ChildOrg(String orgName) {
        this.orgName = orgName;
    }

    @Override
    public void add(Organization org) {
        children.add(org);
    }

    @Override
    public void remove(Organization org) {
        children.remove(org);
    }

    @Override
    public void dispaly(int index) {
        System.out.println("第" + index + "层组织机构" + "---" + orgName);
        for (Organization org: children) {
            org.dispaly(index + 1);
        }
    }
}
```

### 测试

```java
public class CompositeTest {

    public static void main(String[] args) {
        String rootName = "根组织";
        Organization root = new ChildOrg(rootName);

        String firstOrgName = "一级组织";
        Organization first = new ChildOrg(firstOrgName);

        String secondOrgName = "二级组织";
        Organization second = new ChildOrg(secondOrgName);

        String nextOrgName = "一级同级组织";
        Organization nextOrg = new ChildOrg(nextOrgName);

        String nextSecondName = "二级同级组织";
        Organization nextSecond = new ChildOrg(nextSecondName);

        root.add(first);
        first.add(second);
        root.add(nextOrg);
        nextOrg.add(nextSecond);

        root.dispaly(1);
    }
}
```

### 结果

![组合模式示例结果图](../../static/composite-result.png)
## 应用场景

组合模式解耦了客户程序与复杂元素内部结构，从而使客户程序可以像处理简单元素一样来处理复杂元素。所以下面列举的这些场景比较适合使用组合模式。

+ 遇到部分与整体这种概念的应用。

+ 需要创建层次结构并可以在其中以相同的方式对待所有元素的场景。

## 优点

+ 高层模块调用简单。

+ 节点可以自由增减。

## 缺点

在使用组合模式时，其叶子和树枝的声明都是实现类，而不是接口，违反了依赖倒置原则。