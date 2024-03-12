# JavaScript

一般来讲面试的第一轮考察前端基础，作为三件套中实现交互和业务逻辑的语言，是考察的重点。

## 数据类型

### `0.1 + 0.2` 为什么不等于 0.3？

实际上 `0.1 + 0.2` 结果是 `0.30000000000000004`，不等于 `0.3`。
js 设计和 java 很像，js 中只有浮点数，8 字节。
十进制可以准确表示 `0.1`，但是计算机是采用二进制存储，二进制无法准确表示 `0.1` 和 `0.2`。
所以两个存储不精确的数 `0.1` 和 `0.2` 相加的结果也不会是精确的 `0.3`。

### 怎么处理浮点数精度问题？

1. 可以使用第三方库

```javascript
console.log(0.1 + 0.2 === 0.3); // false
console.log(new Decimal('0.1').plus(new Decimal(0.2)).equals(new Decimal(0.3))); // true
```

2. 使用字符串模拟数字相加

```typescript
/**
 * 整数相加
 */
function sum(integer1: string, integer2: string): string {
  let integerResult = '';
  let carry = 0;
  for (let i = 0, len = Math.max(integer1.length, integer2.length); i < len; i++) {
    const sum =
      Number.parseInt(integer1[integer1.length - i - 1] ?? 0) +
      Number.parseInt(integer2[integer2.length - i - 1] ?? 0) +
      carry;
    carry = sum >= 10 ? 1 : 0;
    integerResult = (sum % 10) + integerResult;
  }
  if (carry === 1) {
    integerResult = `1${integerResult}`;
  }
  return integerResult;
}

/**
 * 虑两个小数相加
 */
function plus(num1: number, num2: number): string {
  const str1 = String(num1);
  const str2 = String(num2);

  let [integer1, decimal1] = str1.split('.');
  let [integer2, decimal2] = str2.split('.');

  let longerDecimal: string;
  if (decimal1.length < decimal2.length) {
    longerDecimal = decimal2;
    decimal1 = decimal1.padEnd(decimal2.length, '0');
  } else {
    longerDecimal = decimal1;
    decimal2 = decimal2.padEnd(decimal1.length, '0');
  }

  let decimalResult = sum(decimal1, decimal2);
  let integerResult = sum(integer1, integer2);
  const carried = decimalResult.length !== longerDecimal.length;
  if (carried) {
    // 去除左侧多余的 1
    decimalResult = decimalResult.slice(1);
    integerResult = sum(integerResult, '1');
  }
  return `${integerResult}.${decimalResult}`;
}

console.log(plus(9.1, 11.93)); // 21.o3
```
