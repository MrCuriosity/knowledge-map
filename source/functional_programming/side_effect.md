函数副作用是指，除了返回值以外，还对主调函数产生的附加影响，以下为一些例子:
   - 修改参数
   - 调用/修改函数外的状态，如`window`/`document`
   - API calls
   - console.log()
   - `Math.random() // 不可预测`
