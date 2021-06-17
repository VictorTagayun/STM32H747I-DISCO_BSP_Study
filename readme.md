## Study how to init all the peripherals by BSP Drivers. RCC and UART are the only peripherals initialized in CubeMX with 400Mhz (M7) & 200Mhz (M4) as this is the optimized freq for "most" peripherals espcially the LCD 

## CubeMX Initiallization RCC and UART (minimum)  

RCC =   
	RCC_OscInitStruct.PLL.PLLM = 5;  
	RCC_OscInitStruct.PLL.PLLN = 160;  
	RCC_OscInitStruct.PLL.PLLP = 2;  
	
	
### add BSP for GPIO, Button, Jostick and LED 

Copy these files to BSP inside CM7 driver folder   
	stm32h747i_discovery.c  
	stm32h747i_discovery.h  
	stm32h747i_discovery_conf.h  
	stm32h747i_discovery_errno.h  


### BSP for LCD also needs SDRAM and I2C (for DSI to HDMI, ADV7533)

Combine all BSP for LCD, SDRAM and I2C

copy these to Drivers\BSP\STM32H747I-DISCO in CM7 folder   
	stm32h747i_discovery_bus.c/h   
	stm32h747i_discovery_lcd.c/h  
	stm32h747i_discovery_sdram.c/h  

create Components folder Drivers\BSP in CM7 folder and copy these fodlers inside  
	is42s32800j  
	otm8009a  
	adv7533  
	
add these in paths and symbols  
	Drivers\BSP\Components\ 

edit stm32h7xx_hal_conf.h enable and comment out these  
	#define HAL_DSI_MODULE_ENABLED  
	#define HAL_DMA2D_MODULE_ENABLED  
	#define HAL_LTDC_MODULE_ENABLED  
	#define HAL_SDRAM_MODULE_ENABLED  
	
copy these into Drivers\STM32H7xx_HAL_Driver folder, also link the files into project  
	stm32h7xx_hal_dma2d.c/h  
	stm32h7xx_hal_dsi.c/h  
	stm32h7xx_hal_ltdc.c/h  
	stm32h7xx_hal_ltdc_ex.c/h  
	stm32h7xx_hal_sdram.c/h  
	stm32h7xx_ll_fmc.c/h  
	
copy these files from BSP example to CM7 inc folder   
	is42s32800j_conf.h   
	lcd.h   
	stlogo.h   
	
copy functions from lcd.c	
	static void LCD_SetHint(void)   
	static void LCD_Show_Feature(uint8_t feature)   
	
in main.h add these   
	#include "stm32h747i_discovery_bus.h"  
	#include "stm32h747i_discovery_lcd.h"   
	#include "stm32h747i_discovery_sdram.h"   
	#include "stm32_lcd.h"   
	#include "stlogo.h"  
	#include <stdio.h>  
	
in lcd.c add   
	#include "stlogo.h" 
	
copy Utilities folder from BSP example to CM7 foldert  

add these in paths and symbols   
	Utilities/lcd   


### Test LCD  

	BSP_LCD_Init(0, LCD_ORIENTATION_LANDSCAPE);
	UTIL_LCD_SetFuncDriver(&LCD_Driver);
	UTIL_LCD_SetFont(&UTIL_LCD_DEFAULT_FONT);
	Display_DemoDescription();
	HAL_Delay(2000);

	LCD_SetHint();
	HAL_Delay(2000);

	for(uint32_t feature = 0; feature < 4; feature++)
	{
		LCD_Show_Feature(feature);
		HAL_Delay(1000);
	}


### Test SDRAM 

copy sdram.c to CM7 source

copy is42s32800j_conf.h from BSP example to CM7 inc

in sdram.c, change SDRAM_WRITE_READ_ADDR to SDRAM_DEVICE_ADDR  

in main.c add  
	extern void SDRAM_demo (void);
	extern void SDRAM_DMA_demo (void);

Test SDRAM  
	SDRAM_demo();  
	HAL_Delay(2000);  
	
	
### Test SDRAM DMA = to do, need to add DMA abd IT  

Test SDRAM (to do)  
	SDRAM_DMA_demo();  
	HAL_Delay(2000);
	
	
### Test QSPI  

copy qspi.c to CM7 source   

copy these to Drivers\BSP\STM32H747I-DISCO in CM7 folder   
	stm32h747i_discovery_qspi.c/h 

in main.h add this    
	#include "stm32h747i_discovery_qspi.h"  
	
edit stm32h7xx_hal_conf.h enable and comment out these  
	#define HAL_QSPI_MODULE_ENABLED
	
copy these into Drivers\STM32H7xx_HAL_Driver folder, also link the files into project  
	stm32h7xx_hal_dma2d.c/h
	
create Components folder Drivers\BSP in CM7 folder and copy these fodlers inside  
	mt25tl01g   
	
copy is42s32800j_conf.h from BSP example to CM7 inc


### Test TouchScreen

copy touchscreen.c to CM7 source

copy these to Drivers\BSP\STM32H747I-DISCO in CM7 folder   
	stm32h747i_discovery_ts.c/h

in main.h add this    
	#include "stm32h747i_discovery_ts.h"  
	
edit stm32h7xx_hal_conf.h enable and comment out these  
	#define HAL_QSPI_MODULE_ENABLED
	
copy these into Drivers\STM32H7xx_HAL_Driver folder, also link the files into project  
	stm32h7xx_hal_dma2d.c/h
	
create Components folder Drivers\BSP in CM7 folder and copy these fodlers inside  
	ft6x06
	Common\ts.h
		
copy ft6x06_conf.h from BSP example to CM7 inc




