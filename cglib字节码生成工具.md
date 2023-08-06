### cglib生成的代理类字节码获取

说一个比较笨的方法可以在断点调试的时候，找到那个存放代理类字节码的字节数组，然后在watches窗口
new FileOutputStream("d:\\代理类名.class").write(b),也就是把它写到硬盘上,但是只有debug结束,该文件才有字节,因为debug阶段,这个字节数组还在内存中,debug结束才将字节数组中的内容刷出