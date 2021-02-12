#include <Wire.h>

// minimal Arduino TSL2951 I2C code 
// put together with help from the VERY COMPLETE TSL2591 library by Austria Microsystems
// "TSL2591MI.h" Yet Another Arduino ams TSL2591MI Lux Sensor 
// https://bitbucket.org/christandlg/tsl2591mi/src/master/
// https://www.arduino.cc/reference/en/libraries/tsl2591mi/
// SJ Remington 2/2021

#define TSL2591_ADDR 0x29
#define TSL2591_CMD 0b10000000
#define TSL2591_TRANSACTION_NORMAL 0b00100000
#define TSL2591_MASK_AEN 0b00000010
#define TSL2591_MASK_PON 0b00000001
#define TSL2591_MASK_SRESET 0b10000000
#define TSL2591_MASK_AVALID 0b00000001
// register numbers
#define TSL2591_REG_ENABLE 0x00
#define TSL2591_REG_CONFIG 0x01
#define TSL2591_REG_ID 0x12
#define TSL2591_REG_STATUS 0x13
#define TSL2591_REG_C0DATAL 0x14
#define TSL2591_REG_C0DATAH 0x15
#define TSL2591_REG_C1DATAL 0x16
#define TSL2591_REG_C1DATAH 0x17

int readRegister(uint8_t reg) {  //int return value, because -1 is possible
  Wire.beginTransmission(TSL2591_ADDR);
  Wire.write(TSL2591_CMD | TSL2591_TRANSACTION_NORMAL | reg);
  Wire.endTransmission(false);
  Wire.requestFrom(TSL2591_ADDR, 1);
  return Wire.read();
}

void writeRegister(uint8_t reg, uint8_t value)
{
  Wire.beginTransmission(TSL2591_ADDR);
  Wire.write(TSL2591_CMD | TSL2591_TRANSACTION_NORMAL | reg);
  Wire.write(value);
  Wire.endTransmission();
}

// reset to power on state
void TSL2591_reset(void) {
  writeRegister(TSL2591_REG_CONFIG, TSL2591_MASK_SRESET);
}

// set gain 0 to 3 (low, med, high, max), set integration time 100 to 600 (ms)
void TSL2591_config(uint8_t gain, uint8_t time) {
  writeRegister(TSL2591_REG_CONFIG, (gain << 4 | (time / 100 - 1))); //bits 5:4 and 2:0
}

// get readings from C0 and C1
uint32_t TSL2591_getData(void) {
  uint32_t data = 0;
  uint8_t t;
  t = readRegister(TSL2591_REG_C1DATAH);
  data = t << 8 |  readRegister(TSL2591_REG_C1DATAL);
  data = data << 8 |  readRegister(TSL2591_REG_C0DATAH);
  data = data << 8 |  readRegister(TSL2591_REG_C0DATAL);
  return data;
}
// power on and turn on ambient light sensor
void TSL2591_enable() {
  writeRegister(TSL2591_REG_ENABLE, 3);
}

// register dump for debugging
void TSL2591_regDump() {
    for (int i = 0; i < 0x18; i++) {
    Serial.print("reg "); Serial.print(i);  Serial.print(" "); Serial.println(readRegister(i), HEX);
  }
}

void setup() {
  // put your setup code here, to run once:
  Serial.begin(9600);

  //wait for serial connection to open (only necessary on some boards)
  while (!Serial);

  Wire.begin();
  uint8_t id = readRegister(TSL2591_REG_ID); // read ID register
  if (id != 0x50) {
    Serial.print("ID: ");
    Serial.print(id, HEX);
    Serial.println("? TSL2591 not found");
    while (1);
  }
  TSL2591_reset();
  TSL2591_enable();  //use TSL2591_reset() to disable and power down
  TSL2591_config(1, 100); //medium gain, 100 ms integration time
}

void loop() {
  // put your main code here, to run repeatedly:
  delay(300);
  while((readRegister(TSL2591_REG_STATUS)&1) == 0); //wait for data available
  uint32_t data = TSL2591_getData();
  Serial.print("C0 "); Serial.println( (unsigned int) data&0xFFFF); //visible PD
  Serial.print("C1 "); Serial.println( (unsigned int) (data>>16)&0xFFFF); //IR PD
  Serial.println();
}
