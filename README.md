
# javascript长用方法

## JS数组去重（三种方法）

### 1、
Array.prototype.unique1 = function () {
  var n = []; //一个新的临时数组
  for (var i = 0; i < this.length; i++) //遍历当前数组
  {
    //如果当前数组的第i已经保存进了临时数组，那么跳过，
    //否则把当前项push到临时数组里面
    if (n.indexOf(this[i]) == -1) n.push(this[i]);
  }
  return n;
}

### 2、
Array.prototype.unique2 = function(){
    var n = {},r=[]; //n为hash表，r为临时数组
    for(var i = 0; i < this.length; i++) //遍历当前数组
    {
        if (!n[this[i]]) //如果hash表中没有当前项
        {
            n[this[i]] = true; //存入hash表
            r.push(this[i]); //把当前数组的当前项push到临时数组里面
        }
    }
    return r;
}

### 3、
Array.prototype.unique3 = function()
{
    var n = [this[0]]; //结果数组
    for(var i = 1; i < this.length; i++) //从第二项开始遍历
    {
        //如果当前数组的第i项在当前数组中第一次出现的位置不是i，
        //那么表示第i项是重复的，忽略掉。否则存入结果数组
        if (this.indexOf(this[i]) == i) n.push(this[i]);
    }
    return n;
}

# 1、js数组的深浅拷贝

## 2、js浅拷贝

### 2.1
var sum = [];
for (var i = 0; i < 10; i++) {
  var data = (i + 1) * i;
  sum.push(data);
}
var total = sum;
total[5] = 1;
console.log(total[5]);    //total[5]输出为1
console.log(sum[5]);    //sum[5]输出为1
这时改变total指定下标的值是也会将sum相对应的小标的值改变，应为这时的数组拷贝是浅拷贝

## 3、深拷贝

### 3.1
var copy = function (arr1,arr2) {
   for (var i = 0; i < arr1.length; i++) {
      arr2[i] = arr1[i]
   }
}
var sum = [];
for (var i = 0; i < 10; i++) {
  var data = (i + 1) * i;
  sum.push(data);
}
copy(sum,total)
total[5] = 1;
console.log(total[5]);    //total[5]输出为1
console.log(sum[5]);    //sum[5]输出为30


## 4、js浮点型计算精准度缺失
//加法
function accAdd(arg1,arg2){
    var r1,r2,m;
    try{r1=arg1.toString().split(".")[1].length}catch(e){r1=0}
    try{r2=arg2.toString().split(".")[1].length}catch(e){r2=0}
    m=Math.pow(10,Math.max(r1,r2))
    return (Math.round(arg1*m)+Math.round(arg2*m))/m
}
//减法
function accSub(arg1,arg2){
    var r1,r2,m,n;
    try{r1=arg1.toString().split(".")[1].length}catch(e){r1=0}
    try{r2=arg2.toString().split(".")[1].length}catch(e){r2=0}
    m=Math.pow(10,Math.max(r1,r2));
    n=(r1>=r2)?r1:r2;
    return ((arg1*m-arg2*m)/m).toFixed(n);
}
//乘法
function accMul(arg1,arg2)
{
    var m=0,s1=arg1.toString(),s2=arg2.toString();
    try{m+=s1.split(".")[1].length}catch(e){}
    try{m+=s2.split(".")[1].length}catch(e){}
    return Number(s1.replace(".",""))*Number(s2.replace(".",""))/Math.pow(10,m)
}
//除法
function accDiv(arg1,arg2){
    var t1=0,t2=0,r1,r2;
    try{t1=arg1.toString().split(".")[1].length}catch(e){}
    try{t2=arg2.toString().split(".")[1].length}catch(e){}
    with(Math){
        r1=Number(arg1.toString().replace(".",""))
        r2=Number(arg2.toString().replace(".",""))
        return accMul((r1/r2),pow(10,t2-t1));
    }
}
