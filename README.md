# mine-game-with-atmega
make a game with atmega and a keypad and 4 characteristics 2*16 lcd
i have simulate it proteus i have come to conclusion that it will run until 66th line but doesnt go further from 71th line
/*
 * mine.c
 *
 * Created: 12/6/2018 10:01:23 AM
 * Author: shiraz computer
 */

#include <io.h>
#include <stdlib.h>
#include <alcd.h>
#include <delay.h>
#include <mega32.h>
#include <stdio.h>
int m=0;
int a,b,u,y,o;
char r,k;

int corr;

char arrkey[16]={'0','A','0','0'
                ,'<','*','>','0'
                ,'0','V','0','0'} ; 
unsigned char scan[4]={0XFE,0XFD,0XFB,0XF7};
char c=5;
char keypad(){
 

while(1){
for(r=0;r<4;r++){
PORTB=scan[r];
delay_us(10);
if(PINB.4==0)
c=0;
if(PINB.5==0)
c=1 ;
if(PINB.6==0)
c=2;
if(PINB.7==0)
c=3 ;  }      
if(c!=5){
 k=arrkey[(r*4)+c];
while(PINB.4==1);  while(PINB.5==1); while(PINB.6==1); while(PINB.7==1); 
delay_ms(50);        
return k;}} }

int por;
int n[]={20,23,26,29,32,35,38,41,44};
 int i,j; 
 int d[10][18]={0};
 char key;
 char buf[32];
 

void main(void){
 DDRC=0XFF;
     DDRB=0X0F; 
lable1:
por=0;

for(i=1;20;i++){

a=rand()%16+1;
b=rand()%8+1;
   if(d[a][b]!=9)
 d[a][b]=9;
 
if(d[a][b]==9)
 por++;

 }
     
 for(i=1;8;i++){ 
 
 for(j=1;16;j++){
 
 o=0; 
     
 if(d[i][j]!=9){
 
 if(d[i+1][j]==9)
 o++;
 if(d[i-1][j]==9)
 o++;
 if(d[i][j+1]==9)
 o++;
 if(d[i][j-1]==9)
 o++;
 if(d[i+1][j+1]==9)
 o++;
 if(d[i+1][j-1]==9)
 o++;
 if(d[i-1][j+1]==9)
 o++;
 if(d[i-1][j-1]==9)
 o++;
 d[i][j]=o;}}}
     PORTC=0X01;
     lcd_init(16);    
     u=1;
     y=7;
     lcd_gotoxy(u,y) ;
while (1)
    { key=keypad();
    if(key=='A'){ 
    u++;
    if(u==5)
    u=-3;}
     if(key=='V'){ 
    u--;     
    if(u==-4)
    u=4;}
    if(key=='>'){ 
    y++;   
    if(y==16)
    y=0;}
    if(key=='<'){ 
    y--;
    if(y==-1)
    y=15;}
    if(u==3 ||u==4){
    PORTC=0x00;
    lcd_init(16);    
    lcd_gotoxy(-u+4,y);}
    if(u==1||u==2){
    PORTC=0x01;
    lcd_init(16);
    lcd_gotoxy(-u+2,y);}
    if(u==0||u==-1){
    PORTC=0x02;
    lcd_init(16);
    lcd_gotoxy(-u,y);}
    if(u==-2||u==-3){ 
    PORTC=0x03;
    lcd_init(16);
    lcd_gotoxy(-u-2,y);}   
    if(key=='*'){ 
    if(d[-u+5][y+1]==9){
    sprintf(buf,"%d",d[-u+5][y+1]);
    lcd_puts(buf);
    lcd_puts("LOSER");
    goto lable1; }
    else   { 
    sprintf(buf,"%d",d[-u+5][y+1]);
    lcd_puts(buf);
    corr ++; 
    if(corr==128-n[m]){
    lcd_puts("congrats");
    m++;
    goto lable1;}    }

    
   
    }
}}  
