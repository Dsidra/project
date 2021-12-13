sbit LCD_RS at RE2_bit;
sbit LCD_EN at RE1_bit;
sbit LCD_D0 at RD0_bit;
sbit LCD_D1 at RD1_bit;
sbit LCD_D2 at RD2_bit;
sbit LCD_D3 at RD3_bit;
sbit LCD_D4 at RD4_bit;
sbit LCD_D5 at RD5_bit;
sbit LCD_D6 at RD6_bit;
sbit LCD_D7 at RD7_bit;

sbit LCD_RS_Direction at TRISE2_bit;
sbit LCD_EN_Direction at TRISE1_bit;
sbit LCD_D0_Direction at TRISD0_bit;
sbit LCD_D1_Direction at TRISD1_bit;
sbit LCD_D2_Direction at TRISD2_bit;
sbit LCD_D3_Direction at TRISD3_bit;
sbit LCD_D4_Direction at TRISD4_bit;
sbit LCD_D5_Direction at TRISD5_bit;
sbit LCD_D6_Direction at TRISD6_bit;
sbit LCD_D7_Direction at TRISD7_bit;
float temp ;
char temp_str[5];
int upper_limit=70;
int lower_limit=20;
char upper_limit_str[5];
char lower_limit_str[5];
int temp_cel;

void Read_Temp_in_string(){
temp = ADC_Read(2);
temp_cel = ((temp*5)/10.24);
}


 void Check_Limits_buzzer(){
 if (temp_cel> upper_limit || temp_cel < lower_limit){
 RC1_bit = 1;
 }
 else
 RC1_bit= 0;
 }

 void Heater_init() {
      RC5_bit = 0;
    }
 void Cooler_init() {
    RC2_bit= 0;
 }

 void Heater_check() {
  if(temp_cel < lower_limit)
  RC5_bit = 1;
  else
  RC5_bit = 0;
 }
 void Cooler_check(){
 if (temp_cel > upper_limit)
 RC2_bit = 1;
 else
 RC2_bit= 0;
 }

 void upper_limit_edit() {
  if(RB0_bit == 1)
  upper_limit ++;
  if(RB1_bit == 1)
  upper_limit --;
 }
  void lower_limit_edit() {
  if(RB4_bit == 1)
  lower_limit ++;
  if(RB3_bit == 1)
  lower_limit --;
 }


void main() {
ANSELA= 0xFF;
ANSELB= 0;
ANSELC= 0;
ANSELD= 0;
ANSELE= 0;
TRISA= 0xFF;
TRISB= 0xFF;
TRISC= 0;
TRISD= 0;
TRISE= 0;
 Lcd_Init();
 Heater_init();
 Cooler_init();
 ADC_Init();
 Lcd_Cmd(_LCD_CURSOR_OFF);
 Lcd_Cmd(_LCD_CLEAR);
 Lcd_Out(2,1, "UL:");
 Lcd_Out(2,8,"LL:");
 Lcd_Out(1,1, "Temp:");
 while(1){
 Read_Temp_in_string();
shortToStr(temp_cel, temp_str);
shortToStr(upper_limit,upper_limit_str);
shortToStr(lower_limit,lower_limit_str);
 Lcd_Out(1,6, temp_str);
 Lcd_Out(2,4, upper_limit_str);
 Lcd_Out(2,12, lower_limit_str);
 Check_Limits_buzzer();
 upper_limit_edit();
 lower_limit_edit() ;
 Heater_check();
 Cooler_check();
 }
}
