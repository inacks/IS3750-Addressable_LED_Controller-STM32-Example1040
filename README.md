# IS3750 STM32 Traffic Light Example

This example demonstrates how to use the **IS3750 Addressable LED Controller Chip** with an STM32 microcontroller.  
The project is based on **STM32CubeIDE** and uses the **HAL drivers** from ST's Cube firmware library.

✅ The example was tested using the **Nucleo-C071** evaluation board, but it can be adapted to any STM32 microcontroller using HAL.

> 📦 The `.rar` file in this repository contains the full STM32CubeIDE project.


## 🛒 Buy the IS4310 – Modbus RTU Slave Chip

For more information or to purchase the IS4310, visit:  
👉 [www.inacks.com/is4310](https://www.inacks.com/is4310)

You can also get the evaluation board for the IS3750, compatible with Arduino and Nucleo boards:  
👉 **Kappa3750Ard** – Available at [www.inacks.com/kappa4310ard](https://www.inacks.com/kappa4310ard)

For more products and documentation, visit:  
👉 [www.inacks.com](https://www.inacks.com)



## 📄 Example Code

Below is a simplified version of the example excluding the auto-generated HAL files, for quick reference.

```c
#define IS3750_REGISTER_SHOW        0x00
#define IS3750_REGISTER_LED1_RED    0x01
#define IS3750_REGISTER_LED1_GREEN  0x02
#define IS3750_REGISTER_LED1_BLUE   0x03
#define IS3750_REGISTER_LED2_RED    0x04
#define IS3750_REGISTER_LED2_GREEN  0x05
#define IS3750_REGISTER_LED2_BLUE   0x06
#define IS3750_REGISTER_LED3_RED    0x07
#define IS3750_REGISTER_LED3_GREEN  0x08
#define IS3750_REGISTER_LED3_BLUE   0x09


// Sends brightness value to a specific register of the IS3750.
void writeLedRegister(uint16_t registerAddress, uint8_t bright) {
  uint8_t IS3750_I2C_Chip_Address = 0x12 << 1; // STM32 HAL expects 8-bit I2C address
  HAL_I2C_Mem_Write(&hi2c1, IS3750_I2C_Chip_Address, registerAddress, I2C_MEMADD_SIZE_16BIT, &bright, 1, 1000);
}

// Triggers the IS3750 to update the LED outputs.
void showLEDs(void) {
  uint8_t IS3750_I2C_Chip_Address = 0x12 << 1;
  uint8_t dataToWrite[1] = {1}; // Command to show updated values
  HAL_I2C_Mem_Write(&hi2c1, IS3750_I2C_Chip_Address, IS3750_REGISTER_SHOW, I2C_MEMADD_SIZE_16BIT, dataToWrite, 1, 1000);
}

// Sets all LED registers to 0 (turns off all LEDs).
void clearAllLedRegisters(void) {
  uint8_t IS3750_I2C_Chip_Address = 0x12 << 1;
  uint8_t dataToWrite[1200 * 3] = {0}; // 3600 zeroed bytes
  HAL_I2C_Mem_Write(&hi2c1, IS3750_I2C_Chip_Address, IS3750_REGISTER_LED1_RED, I2C_MEMADD_SIZE_16BIT, dataToWrite, sizeof(dataToWrite), 1000);
}


int main(void)
{
  while (1)
  {
    // Show green on LED1
    clearAllLedRegisters();
    writeLedRegister(IS3750_REGISTER_LED1_GREEN, 5);
    showLEDs();
    HAL_Delay(500);

    // Show yellow on LED2 (Red + Green)
    clearAllLedRegisters();
    writeLedRegister(IS3750_REGISTER_LED2_RED, 5);
    writeLedRegister(IS3750_REGISTER_LED2_GREEN, 5);
    showLEDs();
    HAL_Delay(500);

    // Show blue on LED3
    clearAllLedRegisters();
    writeLedRegister(IS3750_REGISTER_LED3_BLUE, 5);
    showLEDs();
    HAL_Delay(500);
  }
}
```


