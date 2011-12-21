* 2011/12/15 15:11:11
  在Django中使用pdb进行调试
  在需要设置breakpoint的地方设置

  ```python
  import pdb
  pdb.set_trace()
  '''

  常用的调试命令
  h -> help
  l -> list
  a -> argument
* 2011/12/16 13:17:44
django orm 中的QuerySet可以做集合操作，例如取两个QuerySet的并集，可以使用 `q1|q2`

* 2011/12/20 13:16:32
