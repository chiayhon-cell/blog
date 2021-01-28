---
title: 【集合】Collection接口
date: 2021-01-27 21:55:24
tags:
---

# 前言

本篇主要介绍 Collection接口

<!-- more -->

# 底层原理



{% spoiler "MyCollection接口示例" %}



``` java MyCollection接口
package main;

import java.util.Collection;
import java.util.Iterator;
import java.util.Objects;

/**
 * @Description:
 * 概要：
 * MyCollection接口是集合的根接口。集合用于存储一组对象，而这些对象一般称为集合的元素。不同的集合对元素可以有不同的规则，例如，
 * 一些集合可以要求集合里的元素是有序的，或者是不能重复存储，亦或者是两者都要求。
 * 继承链：
 * MyCollection接口一般不提供任何的直接实现，它一般用于定义所有子接口的共有方法，是所有子接口的高度抽象。每个子接口是某种特定的集合的方法规范，
 * 而根接口则是包含了这些集合的所有通用的基本操作。MyCollection接口可以被间接实现，即通过实现子接口间接实现。
 * 初始化：
 * 所有的MyCollection实现类都应该提供两种"标准"构造方法：一个是void(无参数)构造方法，用于创建空的集合;另一个是形参为MyCollection类型的单参
 * 数构造方法，用于创建一个与传入参数有相同元素的新集合。
 * 使用场景：
 * MyCollection接口使用场景多是作为形参传递某种集合(利用多态性),或者是在集合通用方法的使用频率高的地方进行使用(最大普遍性)。
 * 关于异常：
 * MyCollection接口包含修改集合元素的方法，如果实现的集合不支持该操作，则这些方法应该抛出 UnsupportedOperationException。另外，某些
 * 集合对它们的元素会有限制，例如，某些实现会禁止将null加入其集合中。此时若试图将不合法元素加入集合，应该抛出一个非受检异常，通常是
 * NullPointerException 或 ClassCastException。又或者，试图查询集合中是否存在不合法元素时，可以抛出一个异常，或者返回false，这取决于集合
 * 的具体实现。总之对于这样子的异常在此接口的规范中是"可选"的
 * 关于同步：
 * MyCollection不提供同步策略，由每个集合自身来确定同步策略
 * 关于equals方法：
 * 集合中很多方法都是基于equals方法定义的。例如，contains(Object obj) 方法的规范声明：“当且仅当此集合包含至少一个满足
 * (obj==null ? e==null :obj.equals(e)) 表达式的元素 e 时，返回 true。”。但这不是暗指：调用非空参数obj的Collection.contains方法时
 * 一定会调用obj.equals(e)，这里只是解释集合很多方法依赖于equals方法来完成，如果有更好的优化也可以避免使用equals方法，例如可以先比较两元素的
 * 哈希码是否相等。
 *
 * @param: <Element> 集合中元素的类型
 * @Version: 1.0
 * @since: jdk1.8
 */

public interface MyCollection<Element> {
    //===========查询操作===========

    /**
    * @Description: size()方法用于返回此集合中的元素数
    * @return: 此集合中元素的数量
    */
    int size();

    /**
    * @Description: isEmpty()方法用于查询集合是否为空
    * @return: 如果集合没有元素则返回 true
    */
    boolean isEmpty();

    /**
    * @Description: contain(Object obj)方法用于查找是否包含指定元素
    * @param obj: 将要检查是否存在于此集合中的元素
    * @return: 如果此集合包含指定的元素，则返回true
    * @throws ClassCastException: 如果指定元素的类型与此集合不兼容（可选）
    * @throws NullPointerException: 如果指定的元素为null，并且此集合不允许为null的元素（可选)
    */
    boolean contain(Object obj);

    /**
    * @Description: iterator()方法用于获得用于遍历数组的迭代器。
    * @return: 返回一个包含该集合元素的迭代器。
    */
    Iterator<Element> iterator();

    /**
    * @Description: toArray()方法返回一个包含集合所有元素的Object类型的数组
    * @return: Object类型的数组
    */
    Object[] toArray();

    /**
    * @Description: toArray(Type[] array)方法指定一个Type类型的数组，将集合中的所有元素存入其中(要求集合中的元素也是Type类型或其子类型)
     * ,若指定数组不能容纳集合中的所有元素，则将创建一个长度与集合元素数量相等的数组，然后将所有集合元素存入其中并返回。
    * @param array: 指定一个用于存放所有元素的Type类型的数组
    * @return: 存放着所有元素的数组
    */
    <Type> Type[] toArray(Type[] array);


    //===========修改操作===========

    /**
    * @Description: add(Element element)方法用于确保集合中包含指定元素。如果添加成功，则返回true。如果集合不允许元素重复,且集合中已经包
    * 含指定元素，此时返回false，除此原因之外，集合因为其他原因而拒绝添加一个元素，则必须抛出一个异常。
    * @param element: 确保要存在于此集合中的元素
    * @return: 是否添加成功
    * @throws: UnsupportedOperationException
    * @throws UnsupportedOperationException: 如果此集合不支持添加操作
    * @throws ClassCastException: 如果该元素类型与集合指定类型不兼容
    * @throws NullPointerException: 如果指定的元素为null，并且此集合不允许使用null元素
    * @throws IllegalArgumentException: 如果添加非法元素到此集合中
    * @throws IllegalStateException: 如果由于插入限制当前无法添加元素
    */
    boolean add(Element element);

    /**
    * @Description: remove(Element element)方法用于删除元素。如果存在指定元素，则从此集合中删除指定元素，若删除成功则返回 true。
    * @param element: 要从此集合中删除的元素(如果存在)
    * @return: 是否删除成功
    */
    boolean remove(Element element);


    //===========批量操作===========

    /**
    * @Description: containAll(Collection<?> collection)用于查询集合中是否包含指定集合的所有元素
    * @param collection: 指定的集合
    * @return: boolean 如果此集合包含指定集合的所有元素，则为true
    */
    boolean containAll(Collection<?> collection);

    /**
    * @Description: addAll(Collection<? extends Element> collection)用于向集合中添加指定集合的所有元素
    * @param collection:  指定的集合
    * @return: 如果此集合由于调用而更改，则为true
    */
    boolean addAll(Collection<? extends Element> collection);

    /**
    * @Description: removeAll(Collection<?> collection)用于从集合中删除同时存在于指定集合中的元素
    * @param collection: 指定的集合
    * @return: 如果此集合由于调用而更改，则为true
    */
    boolean removeAll(Collection<?> collection);


    /**
    * @Description: remainAll(Collection<Element> collection)用于从此集合中删除所有未包含在指定集合中的元素
    * @param collection:  指定的集合
    * @return: 如果此集合由于调用而更改，则为true
    */
    boolean remainAll(Collection<?> collection);

    /**
    * @Description: clear()用于清空集合，执行此操作后集合将没有元素
    */
    void clear();

    //===========比较与哈希===========

    /**
    * @Description: equals(Object obj)方法用于比较此集合与指定对象是否相等。
    * 若是开发者直接实现MyCollection接口(即不是MyList接口或者MySet接口的实现类),不建议重写equals方法，建议直接使用Object方法里的equals方
    * 法。
    * 在Object.equals方法的常规协定声称:相等必须是对称的(a.equals(b)的结果应该与b.equals(a)相同)。在MyList.equals方法和MySet方法的
     * 常规协定声称MyList集合只能和MyList集合相等，MySet集合只能和MySet集合相等。因此，对于一个不实现MyList集合与MySet集合的Collection
     * 集合，当Collection集合的元素与MyList的和MySet的元素进行比较时，常规的equals方法必须返回false，也就是说，不存在一个既实现MyList
     * 接口又实现MySet接口的实现类。
    * @param obj: 该集合进行相等性比较的对象
    * @return: boolean
    */
    @Override
    boolean equals(Object obj);

    /**
    * @Description: MyCollection中没有对hashCode方法有常规协定，所以应该遵守Object.hashCode的常规协定。
    * @return: 返回此集合的哈希值
    */
    @Override
    int hashCode();
}

```



{% endspoiler %}

AbstractMyCollection为MyCollection的抽象实现类

{% spoiler "AbstractMyCollection接口示例"%}

``` java AbstractMyCollection接口
package main;

import java.util.Collection;
import java.util.Iterator;
import java.util.Objects;

/**
 * @Description: 此类提供了MyCollection接口的基本实现，以最大程度地减少实现此接口所需的工作。
 * 要实现不可修改的集合，只需扩展此类并为iterator和size方法提供实现。 （由iterator方法返回的迭代器必须实现hasNext和next 。）
 * 要实现可修改的集合，程序员必须另外重写此类的add方法（否则将抛出UnsupportedOperationException ），并且iterator方法返回的迭代器必须另外
 * 实现其remove方法。
 * 根据Collection接口规范中的建议，程序员通常应提供一个返回值为void（无参数）和Collection构造函数。
 * 此类中每个非抽象方法的文档都详细描述了其实现。 如果正在实现的集合允许更有效的实现，则可以覆盖这些方法中的每一个
 * @Version: 1.0
 * @since: jdk1.8
 */

public abstract class AbstractMyCollection<Element> implements MyCollection<Element> {

    /**
     * @Description: 构造函数，隐式调用
     */
    protected AbstractMyCollection() {
    }


    //=============查询操作===============


    /**
     * @Description: size()方法用于返回此集合中的元素数
     * @return: 此集合中元素的数量
     */
    @Override
    public abstract int size();

    /**
     * @Description: isEmpty()方法用于查询集合是否为空
     * @return: 如果集合没有元素则返回 true
     */
    @Override
    public boolean isEmpty() {
        return false;
    }

    /**
     * @param obj : 将要检查是否存在于此集合中的元素,
     * @throws ClassCastException:   如果指定元素的类型与此集合不兼容（可选）
     * @throws NullPointerException: 如果指定的元素为null，并且此集合不允许为null的元素（可选)
     * @Description: 此实现对集合中的元素进行迭代，依次检查每个元素与指定元素的相等性。
     * @return: 如果此集合包含指定的元素，则返回true
     */
    @Override
    public boolean contain(Object obj) {
        //获取迭代器
        Iterator<Element> iterator = iterator();

        if (obj == null) {
            //指定元素为null
            while (iterator.hasNext()) {
                if (iterator.next() == null) {
                    //遍历的集合中有元素为null
                    return true;
                }
            }
        } else {
            //指定元素不为null
            while (iterator.hasNext()) {
                if (iterator.equals(obj)) {
                    //遍历的集合中有与指定元素相同的元素
                    return true;
                }
            }
        }

        return false;

    }

    /**
     * @Description: iterator()方法用于获得用于遍历数组的迭代器。
     * @return: 返回一个包含该集合元素的迭代器。
     */
    @Override
    public abstract Iterator<Element> iterator();

    /**
     * @Description: toArray()方法返回一个包含集合所有元素的Object类型的数组
     * @return: Object类型的数组
     */
    @Override
    public Object[] toArray() {
        //预估返回数组长度为当前集合的size

        //创建迭代器

        //假设数组刚好能装下集合中所有元素

        //如果集合元素少于预期，提前结束，返回数组

        //如果元素多于于预期，继续遍历，直至遍历完返回数组副本
        return new Object[0];
    }


    /**
     * @param array : 指定一个用于存放所有元素的Type类型的数组
     * @Description: toArray(Type[] array)方法指定一个Type类型的数组，将集合中的所有元素存入其中(要求集合中的元素也是Type类型或其子类型)
     * ,若指定数组不能容纳集合中的所有元素，则将创建一个长度与集合元素数量相等的数组，然后将所有集合元素存入其中并返回。
     * @return: 存放着所有元素的数组
     */
    @Override
    public Object[] toArray(Object[] array) {
        //预估返回数组长度：判断指定数组的长度是否能容纳集合中的所有元素，若不能则需进行加宽操作

        //假设数组刚好能装下集合中所有元素

        //如果集合元素少于预期，提前结束，并将后续元素置为null，返回数组

        //如果元素多于于预期，继续遍历，直至遍历完返回数组副本
        return new Object[0];
    }




    //=============修改操作===============


    /**
     * @param o : 确保要存在于此集合中的元素
     * @Description: add方法用于确保集合中包含指定元素。如果添加成功，则返回true。如果集合不允许元素重复,且集合中已经包
     * 含指定元素，此时返回false，除此原因之外，集合因为其他原因而拒绝添加一个元素，则必须抛出一个异常。
     * 关于此实现：
     * AbstractMyCollection类未定义底层存储元素的数据结构，无法实现add方法
     * 此实现始终抛出 UnsupportedOperationException(不支持操作异常)
     * @return: 是否添加成功
     */
    @Override
    public boolean add(Object o) {
        throw new UnsupportedOperationException("不支持add方法");
    }

    /**
     * @param o : 要从此集合中删除的元素(如果存在)
     * @Description: remove(Element element)方法用于删除元素。如果存在指定元素，则从此集合中删除指定元素，若删除成功则返回 true。
     * 关于此实现：
     * 此实现遍历集合以查找指定的元素。 如果找到该元素，则使用迭代器的remove方法从集合中删除该元素。
     * 请注意，如果此集合的iterator方法返回的迭代器未实现remove方法并且此集合包含指定的对象，则此实现将引发UnsupportedOperationException 。
     * @return: 是否删除成功
     */
    @Override
    public boolean remove(Object o) {
        //创建迭代器
        Iterator<Element> it = iterator();
        if (o == null) {
            //如果指定删除元素为null
            while (it.hasNext()) {
                if (it.next() == null) {
                    //且集合中元素存在元素为null，进行删除
                    it.remove();
                    return true;
                }
            }
        } else {
            //如果指定删除元素不为null
            while (it.hasNext()) {
                if (o.equals(it.next())) {
                    //且集合中存在元素与指定元素相等,进行删除
                    it.remove();
                    return true;
                }
            }
        }
        //否则删除失败
        return false;
    }


    //=========批量操作==========


    /**
     * @param collection : 检查是否包含在此集合中的集合
     * @Description: containAll(Collection < ? > collection)用于查询集合中是否包含指定集合的所有元素
     * 关于此实现:
     * 此实现迭代指定的集合，依次检查迭代器返回的每个元素以查看其是否包含在此集合中。 如果如此包含所有元素，则返回true ，否则返回false
     * @return: boolean 如果此集合包含指定集合的所有元素，则为true
     */
    @Override
    public boolean containAll(Collection<?> collection) {
        //遍历集合每个元素
        for (Object element : collection) {
            if (!contain(element)) {
                //只要有一个元素不包含,返回false
                return false;
            }
        }

        //否则返回true
        return false;
    }

    /**
     * @param collection :  指定的集合
     * @Description: addAll(Collection < ? extends Element > collection)用于向集合中添加指定集合的所有元素
     * 关于此实现：
     * 此实现迭代指定的集合，并将迭代器返回的每个对象依次添加到此集合。
     * 请注意，此实现将引发UnsupportedOperationException 除非重写了add方法。
     * @return: 如果此集合由于调用而更改，则为true
     */
    @Override
    public boolean addAll(Collection<? extends Element> collection) {
        //定义一个变量表示集合是否发生修改
        boolean isModifySuccess = false;
        for (Element element : collection) {
            if (add(element)) {
                //如果add方法执行成功，则将集合的修改状态变量设为true
                isModifySuccess = true;
            }
        }

        //返回集合的修改状态
        return isModifySuccess;
    }

    /**
     * @param collection : 指定的集合
     * @Description: removeAll(Collection < ? > collection)用于从集合中删除同时存在于指定集合中的元素
     * 关于此实现:
     * 此实现遍历此集合，依次检查迭代器返回的每个元素以查看其是否包含在指定的集合中。 如果包含了它，则使用迭代器的remove方法将其从此集合中删除。
     * 请注意，如果由迭代器方法返回的迭代器未实现remove方法，并且此集合包含一个或多个与指定集合相同的元素，则此实现将引发UnsupportedOperationException。
     * @return: 如果此集合由于调用而更改，则为true
     */
    @Override
    public boolean removeAll(Collection<?> collection) {
        //指定的集合不能为空，否则抛出空指针异常
        Objects.requireNonNull(collection);
        //定义一个变量表示集合是否发生修改
        boolean isModifySuccess = false;
        //定义一个迭代器用于遍历集合
        Iterator<?> iterator = iterator();
        while (iterator.hasNext()) {
            if (collection.contains(iterator.next())) {
                //如果集合中的元素同时存在于指定的集合中，则进行删除
                iterator.remove();
                isModifySuccess = true;
            }
        }
        //返回集合修改状态
        return isModifySuccess;
    }

    /**
     * @param collection :  指定的集合
     * @Description: remainAll(Collection < ? > collection)用于从此集合中删除所有未包含在指定集合中的元素
     * 关于此实现:
     * 此实现遍历此集合，依次检查迭代器返回的每个元素以查看其是否包含在指定的集合中。 如果没有包含，则使用迭代器的remove方法将其从此集合中移除。
     * 请注意，如果iterator方法返回的迭代器未实现remove方法，并且此集合包含一个或多个指定集合中不存在的元素，则此实现将引发UnsupportedOperationException。
     * @return: 如果此集合由于调用而更改，则为true
     */
    @Override
    public boolean remainAll(Collection<?> collection) {
        //指定的集合不能为空，否则抛出空指针异常
        Objects.requireNonNull(collection);
        //定义一个变量表示集合是否发生修改
        boolean isModifySuccess = false;
        //定义一个迭代器用于遍历集合
        Iterator<?> iterator = iterator();
        while (iterator.hasNext()) {
            if (!collection.contains(iterator.next())) {
                //如果集合中的元素不存在于指定的集合中，则进行删除
                iterator.remove();
                isModifySuccess = true;
            }
        }
        //返回集合修改状态
        return isModifySuccess;
    }

    /**
     * @Description: clear()用于清空集合，执行此操作后集合将没有元素
     * 关于此实现：
     * 此实现遍历此集合，使用Iterator.remove操作删除每个元素。 大多数实现可能会选择重写此方法以提高效率。
     * 请注意，如果此集合的迭代器方法返回的迭代器未实现remove方法且该集合为非空，则此实现将引发UnsupportedOperationException 。
     */
    @Override
    public void clear() {
        //定义一个该集合的迭代器
        Iterator<Element> iterator = iterator();
        while (iterator.hasNext()){
            //如果元素存在，则将其从集合中删除
            iterator.next();
            iterator.remove();
        }
    }


    //=========比较与哈希==========


    /**
     * @param obj : 该集合进行相等性比较的对象
     * @Description: equals(Object obj)方法用于比较此集合与指定对象是否相等。
     * 若是开发者直接实现MyCollection接口(即不是MyList接口或者MySet接口的实现类),不建议重写equals方法，建议直接使用Object方法里的equals方
     * 法。
     * 在Object.equals方法的常规协定声称:相等必须是对称的(a.equals(b)的结果应该与b.equals(a)相同)。在MyList.equals方法和MySet方法的
     * 常规协定声称MyList集合只能和MyList集合相等，MySet集合只能和MySet集合相等。因此，对于一个不实现MyList集合与MySet集合的Collection
     * 集合，当Collection集合的元素与MyList的和MySet的元素进行比较时，常规的equals方法必须返回false，也就是说，不存在一个既实现MyList
     * 接口又实现MySet接口的实现类。
     * @return: boolean
     */

    @Override
    public abstract boolean equals(Object obj);

    /**
     * @Description: MyCollection中没有对hashCode方法有常规协定，所以应该遵守Object.hashCode的常规协定。
     * @return: 返回此集合的哈希值
     */
    @Override
    public abstract int hashCode();

    //=========字符串转换==========


    /**
     *
     * @Description: 返回此集合的字符串表示形式。
     * 关于此实现：
     * 字符串表示形式包含一个集合元素的列表，这些元素按其迭代器返回的顺序排列，并括在方括号（ “ []” ）中。
     * 相邻元素由字符“，” （逗号和空格）分隔。 元素通过String.valueOf(Object)转换为字符串
     * @return: 对象的字符串表示形式。
     */
    @Override
    public String toString() {
        //定义一个该集合的迭代器
        Iterator<Element> iterator = iterator();
        //如果集合里没有元素，直接返回 []
        if (!iterator.hasNext()){
            return "[]";
        }

        //创建字符串变量
        StringBuilder stringBuilder = new StringBuilder();
        stringBuilder.append("[");
        //将集合里的元素逐个进行拼接
        while (true){
            //将元素进行拼接
            Element element = iterator.next();
            stringBuilder.append(element);
            //如果没有下一个元素，则将字符串返回
            if (! iterator.hasNext()){
                return stringBuilder.append(']').toString();
            }
            //否则将分隔符逗号','进行拼接
            stringBuilder.append(',').append(' ');
        }
    }
}
```

{% endspoiler %}