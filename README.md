# STM8 SW I2C

* Software Master `I2C` library for `STM8`
* Works blocking way and can be interrupted
* Need "delay microsecond" function to work, compatible with [delay.h](https://github.com/gripennn/stm8-delay/blob/main/delay.h)

## User manual

* In file `swi2c.h` change macros to define your own __GPIOs__ (Pins)

```c
#define SCL_GPIO  GPIOC
#define SCL_PIN   GPIO_PIN_x
#define SDA_GPIO  GPIOC
#define SDA_PIN   GPIO_PIN_x
```

* If you need __slower communication__, you can change also timing macros in `swi2c.h`

```c
#define SWI2C_START_STOP_TIME      5
#define SWI2C_SDA_SETUP_TIME       5
#define SWI2C_SDA_HOLD_TIME        5
#define SWI2C_SCL_HALFPERIOD_TIME  5
```

* ⚠ Do not forget call `swi2c_init()` before use any I2C function!

## Functions

```c
void swi2c_init(void);
```

* Software I2C initialization
* It is required to call this function before calling the functions mentioned below

```c
uint8_t swi2c_test_slave(uint8_t slvaddr);
```

* Checks ACK bit for selected slave address
* Checks if slave present on bus

```c
uint8_t swi2c_write_buf(uint8_t slv_addr, uint8_t address, uint8_t* data, uint16_t num);
```

* Writes 1 address byte and `num` data bytes to slave with selected slave address
* Sequence is:

| SLA + W | Data        |
| ------- | ----------- |
| 1 byte  | `num` bytes |

```c
uint8_t swi2c_read_buf(uint8_t slv_addr, uint8_t address, uint8_t* data, uint16_t num);
```

* Read `num` databytes from selected "data address" from slave with selected slave address
* Sequence is:

| SLA + W | RST | SLA + R | Data        |
| ------- | --- | ------- | ----------- |
| 1 byte  |     | 1 byte  | `num` bytes |

```c
uint8_t swi2c_recover(void);
```

* Try to recover "stuck" I2C bus.
* It simply tick by SCL, wait and observe if SDA is released.
* If yes it generate STOP sequence and returns.

---

All function return statuses (timeout, no ACK, bus failure etc)
