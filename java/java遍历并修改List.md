#### java遍历并修改List

- 普通遍历

```
List<String> list = new ArrayList<>();
list.add("111");
list.add("222");
list.add("333");
// 1. 普通遍历方式
for (int i = 0; i < list.size(); i++) {
 	System.out.println(list.get(i));
}
// 2.增强的for循环
for (String item : list){
    System.out.println(item);
}
```

- 遍历并修改

```
List<SysUser> list = userService.selectUserList(user);
for (SysUser item : list){
    System.out.println(item.getUserName());
    item.setUserName("修改后的用户名");
}
```



