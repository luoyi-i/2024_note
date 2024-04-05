单步 1 us
esp32 单步 1us
```
    u32Time1 = micros();
    u32Time2 = micros();
    log("time step = %ld\n", u32Time1);
    log("time step = %ld\n", u32Time2);
    log("time step = %ld\n", (u32Time2-u32Time1));
```
结果：
```
time step = 82734
time step = 82735
time step = 1