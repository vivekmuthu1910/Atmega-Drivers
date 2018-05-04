# I2C Driver
I2C driver enables user to use TWI interface to communicate with other *μC* or sensors having I2C interface.

## Master mode
* In master mode first set the I2C speed using `I2C_Set_Speed(speed_in_Hz)` function.
* Slave address in master mode points to the address of slave device attached to this *μC*.

### Transmit mode
Send data to a slave using
`I2C_Master_Transmit(Slave_Address, Pointer_to_Data_to_Transmit, Number_of_Bytes_to_transfer)`.
```c
char data_char[]="Hello World";
int data_int[] = {5,6,18,256,1234};
float data_float = 3.65;

char slave_ad = 0x35;

I2C_Set_Speed(100000);

I2C_Master_Transmit(slave_ad, data_char, 11 * sizeof(char));
while(I2C_flag & I2C_Busy){}

I2C_Master_Transmit(slave_ad, data_int, 5 * sizeof(int));
while(I2C_flag & I2C_Busy){}

I2C_Master_Transmit(slave_ad, &data_float, 1 * sizeof(float));
while(I2C_flag & I2C_Busy){}
```

### Receive mode
Receive data from a slave using
`I2C_Master_Receive(Slave_Address, Pointer_to_Data_to_Receive, Number_of_Bytes_to_Receive)`.
```c
char data_char[10];
int data_int[5];
float data_float;

char slave_ad = 0x35;

I2C_Set_Speed(100000);

I2C_Master_Receive(slave_ad, data_char, 10 * sizeof(char));
while(I2C_flag & I2C_Busy){}

I2C_Master_Receive(slave_ad, data_int, 5 * sizeof(int));
while(I2C_flag & I2C_Busy){}

I2C_Master_Receive(slave_ad, &data_float, 1 * sizeof(float));
while(I2C_flag & I2C_Busy){}
```

## Slave Mode
* In slave mode this *μC* doesn't control the SCL line. Hence no need to set the speed.
* Slave address in slave mode is the address of this *μC* which can be addressed by some other master in the line.
* General call address is a broadcast address `0x00`. If general call enable is used, the *μC* will respond to general call address.

### Transmit mode
Send data to a Master when addressed using
`I2C_Slave_Transmit(Slave_Address, Pointer_to_Data_to_Transmit, Number_of_Bytes_to_transfer)`.
```c
char data_char[]="Hello World";
int data_int[] = {5,6,18,256,1234};
float data_float = 3.65;

char slave_ad = 0x35;

I2C_Slave_Transmit(slave_ad, data_char, 11 * sizeof(char));
while(I2C_flag & I2C_Busy){}

I2C_Slave_Transmit(slave_ad, data_int, 5 * sizeof(int));
while(I2C_flag & I2C_Busy){}

I2C_Slave_Transmit(slave_ad, &data_float, 1 * sizeof(float));
while(I2C_flag & I2C_Busy){}
```

### Receive mode
Receive data from a Master when addressed using
`I2C_Slave_Receive(Slave_Address, Pointer_to_Data_to_Transmit, Number_of_Bytes_to_transfer)`.
```c
char data_char[5];

char slave_ad = 0x35;

I2C_Slave_Receive(slave_ad, data_char, 5 * sizeof(char));
while(I2C_flag & I2C_Busy){}

//If General Call is required
I2C_Slave_Receive(slave_ad | I2C_General_Call_En, data_char, 5 * sizeof(char));
while(I2C_flag & I2C_Busy){}
```
