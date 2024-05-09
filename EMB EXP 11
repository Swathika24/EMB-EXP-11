/*SCL   ---->  P0.23
  SDA   ---->  P0.24*/

#include <LPC17xx.H>
#include "GLCD.H"
#define __FI 1									/*	Font Index (0 = 6x8, 1 = 16x24)    */

#define SCL(x) ((x) ? (LPC_GPIO0->FIOSET = 1<<23) : (LPC_GPIO0->FIOCLR = 1<<23));delay()
#define SDA(x) ((x) ? (LPC_GPIO0->FIOSET = 1<<24) : (LPC_GPIO0->FIOCLR = 1<<24));delay()

#define I2C_SDA    ((LPC_GPIO0->FIOPIN & 0x01000000) >> 24)
/* 
The I2C address of U1,U2,U3 and U4 depend on the IC palced on the interface. 
Addrss for PCF8574AP is 0x7X and for PCF8574P is 0x4X. 
So read the part number from the device and define in the below line.
Make sure all the four IC part nos are same.
*/
#define PCF8574AP		// Specify the U1,U2,U3&U4 part number by reading from interface.

#ifdef PCF8574AP	 	/* If Part number is PCF8574AP then assign the address*/
	#define U1  0x70
	#define U2  0x72
	#define U3  0x74
	#define U4  0x76
#endif

#ifdef PCF8574P			/* If Part number is PCF8574P then assign the address*/
	#define U1  0x40
	#define U2  0x42
	#define U3  0x44
	#define U4  0x46
#endif
    		
unsigned char Recved_Data[7];  
unsigned char lookup[10] = {0xC0,0xF9,0xA4,0xB0,0x99,0x92,0x82,0xF8,0x80,0x90};
						/* Lookup table for display code of 0 to 9 digits on LED */
unsigned long SDA =0;


void LED_display(unsigned char , unsigned char);
void Send_Ack(void);
void Send_Start(void);
void Send_Data(unsigned char);
void Send_Stop(void);
unsigned char Recv_Data(void);
void Recv_Ack(void);
void Delay(void);		

void LED_display(unsigned char Slv_Addr, unsigned char Data)
{
	Send_Start();		   					/* Send START Signal */
	
	Send_Data(Slv_Addr);   					/* Send Slave address */
	Recv_Ack();								/* Receive Acknoledge */

	Send_Data(Data);   						/* Send Data */
	Recv_Ack();								/* Receive Acknoledge */

	Send_Stop();							/* Send STOP Signal */ 
	
}
void Delay()							    /* Delay Routine */
{
	unsigned int j,i;
	   	
	for(i=0; i<10; i++)
		for(j=0; j<=15; j++);

}
void delay()
{
	unsigned int i;
	for(i=0;i<100;i++);
}
void Send_Start()
{
	
	SCL(1);								/* Make Clock line High */
	SDA(1);								/* Make Data line High */
	SDA(0);								/* Make Data line Low */
	SCL(0);								/* Make Clock line Low */

}


void Send_Data(unsigned char dat)
{
	unsigned char i;

	SCL(0); 								/* Make Clock line Low */				
	for(i=8; i>0; i--)	 	
	{
		SDA(((dat >> (i-1)) & 0x01)); 		/* Place the Data */
		delay();
		SCL(1);						 	/* Make Clock line High */
		SCL(0); 							/* Make Clock line Low */
	}
}



void Recv_Ack()
{
	
//	SDA(1);								/* Initialize Data pin as as Input */
	SCL(1);								/* Make Clock line High */
	while(I2C_SDA);								/* Wait till Acknoledge received */
	SCL(0);  								/* Make Clock line Low */

}

void Send_Stop()
{
	SDA(0);
	SCL(1);								/* Make Clock line High */
	SDA(0);								/* Make Data line Low */
	SDA(1);								/* Make Data line High */
	SCL(0);
}
main()
{
	LPC_SC->PCONP       = 1 << 15; /* POWER TO PORTS*/
	LPC_GPIO0->FIODIR   = 3 << 23; /* P0.23 AND P0.24 AS SCL AND SDA OUTPUTS*/
	#ifdef __USE_LCD
  GLCD_Init();                               /* Initialize graphical LCD      */
  GLCD_Clear(White);                         /* Clear graphical LCD display   */
  GLCD_SetBackColor(Blue);
  GLCD_SetTextColor(White);
  GLCD_DisplayString(0, 0, __FI, "        ESA         ");
  GLCD_DisplayString(1, 0, __FI, "     Bangalore      ");
  GLCD_DisplayString(2, 0, __FI, "  www.esaindia.com  ");
  GLCD_SetBackColor(White);
  GLCD_SetTextColor(Blue);
  GLCD_DisplayString(6, 0, __FI, " I2C SEVEN SEGMENT  ");
	 
#endif

	while(1)
	{	
		
		delay();

		LED_display(U4,0x88);			/* Display 'A' */
		LED_display(U3,0x92);			/* Display 'S' */
		LED_display(U2,0x86);		  	/* Display 'E' */
	  LED_display(U1,0xBF);
		
		Delay();
		
	}
}
