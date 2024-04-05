
yunque
|type|编码前|----|编码后|
|----|----|----|----|
|preamble|16bits||32bits, Manchester 0 = 10|
|sync  | 9Bits||18bits|
|wakeID| 16Bits|||
|CMD   | 8*3Bits|||

6 bytes incldue CMD

Tbit = 01 解码后的
sync  111000-101100-110010

发送的数据
4*8 bits 10101010 = 0XAA
18bits   111000-101100-110010



频率 3.9*2

每一位的长度   1/(3.9*2) = 1/7.8 = 128.205 us


125K 载波用软件加在正电平上
