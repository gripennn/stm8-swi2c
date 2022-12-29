# stm8-swi2c
Software Master I2C library for STM8

need "delay microsecond" function to work, compatible with: https://github.com/gripennn/stm8-delay/blob/main/delay.h

works blocking way and can be interrupted

"user manual"

in file swi2c.h change macros

#define SCL_GPIO GPIOC

#define SCL_PIN GPIO_PIN_x

#define SDA_GPIO GPIOC

#define SDA_PIN GPIO_PIN_x

to define your own GPIOs (Pins), if you need slower communication, you can change also timing macros.

Do not forget call **swi2c_init()** before use any i2c function.

Functions:

**swi2c_test_slave()** - checks ACK bit for selected slave address - checks if slave present on bus

**swi2c_write_buf()** - writes 1 address byte and "num" data bytes to slave with selected slave address. Sequence is: SLA+W - 1B(Address) - num*1B(data)

**swi2c_read_buf()** - read "num" databytes from selected "data address" from slave with selected slave address. Sequence is:  SLA+W - 1B(Address) - RST - SLA+R + num*1B(data)

**swi2c_recover()** - try to recover "stuck" I2C bus. It siply tick by SCL, wait and observe if SDA is released. If yes it generate STOP sequence and returns.

All function return statuses (timeout, no ACK, bus failure etc)




