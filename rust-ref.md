# Rust Ref


## 引用类型

- &
- AsRef<T>
- Deref::Target

## 引用比较

- & 

  地址

- Deref::Target

  用作智能指针  

  使用场景:

  - *v 显示解引用
  - Deref 协变 (引用隐式转换: 如参数，返回值)

- AsRef<T>

  Used to do a *cheap* reference-to-reference conversion  
  If you need to do a *costly* conversion it is better to 
  implement `From` with type `&T` or write a custom function.

  By using trait bounds we can accept arguments of different types 
  as long as they can be converted to the specified type T.

  > note: AsRef auto-dereferences if the inner type is a reference 
  > or a mutable reference (e.g.: foo.as_ref() will work the same
  > if foo has type &mut Foo or &&mut Foo)

  > `auto-dereferences` 方式, 值得借鉴

  ```Rust
  impl<T: ?Sized, U: ?Sized> const AsRef<U> for &T
  where
      T: ~const AsRef<U>,
  {
      #[inline]
      fn as_ref(&self) -> &U {
	  <T as AsRef<U>>::as_ref(*self)
      }
  }
  ```

- From / Into 类型转换

  Used to do value-to-value conversions while consuming the input value. 
  It is the reciprocal of `Into`.

  实现 From 会自动实现 Into, 优先实现 From
