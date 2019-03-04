# hello-boys
this is a test

$ git clone git@github.com:hirocastest/Hello-World.git
Cloning into 'Hello-World'...
remote: Counting objects: 3, done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (3/3), done.
$ cd Hello-World

void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin)
{
    if(KEY1 == 0)			//×ó±ß°´¼üK3ÉèÖÃ²ÎÊý
    {
        HAL_Delay(50);
        if(KEY1 == 0)
        {
            HAL_TIM_Base_Stop_IT(&TIM3_InitStruct);
            DIO0_DisableInterrupt();
            static uint8_t t = 6;
            t++;
            switch(t)
            {
            case 7:
            {
                G_LoRaConfig.SpreadingFactor = SF07;
                MX_TIM3_Init_Ms(1000);
                break;
            }
            case 8:
            {
                G_LoRaConfig.SpreadingFactor = SF08;
                MX_TIM3_Init_Ms(1000);
                break;
            }
            case 9:
            {
                G_LoRaConfig.SpreadingFactor = SF09;
                MX_TIM3_Init_Ms(1500);
                break;
            }
            case 10:
            {
                G_LoRaConfig.SpreadingFactor = SF10;
                MX_TIM3_Init_Ms(2000);
                break;
            }
            case 11:
            {
                G_LoRaConfig.SpreadingFactor = SF11;
                MX_TIM3_Init_Ms(3000);
                break;
            }
            case 12:
            {
                G_LoRaConfig.SpreadingFactor = SF12;
                MX_TIM3_Init_Ms(4000);
                break;
            }
            default :
                break;
            }
            OLED_ShowNum(18, 12, t, 2, 12);
            OLED_Refresh();
            if(t > 11)
            {
                t = 6;
            }
        }
    }

    if(KEY2 == 0)			//ÓÒ±ß°´¼üK2¿ªÊ¼Í¨Ñ¶
    {
        HAL_Delay(50);
        if(KEY2 == 0)
        {

            HAL_TIM_Base_Stop_IT(&TIM3_InitStruct);
            DIO0_DisableInterrupt();
            static int8_t j = 0;
            j++;
            G_LoRaConfig.LoRa_Freq = Fre[j];
            OLED_ShowNum(48, 0, G_LoRaConfig.LoRa_Freq, 9, 12);
            OLED_Refresh();
            if(j >= 4)
            {
                j = -1;
            }
        }
    }
    if(KEY3 == 0)
    {
        HAL_Delay(50);
        if(KEY3 == 0)
        {
            if(SX127X_Lora_init() != NORMAL)	 //ÎÞÏßÄ£¿é³õÊ¼»¯
            {
                while(1)
                {
                    OLED_Clear();
                    OLED_ShowString(0, 0, "ERoR", 24);
                   OLED_Refresh();
                }
            }
            T_Cnt = 0;
            R_Cnt = 0;
            E_Cnt = 0;
            OLED_ShowNum(12, 44, T_Cnt, 3, 12);
            OLED_ShowNum(48, 44, R_Cnt, 3, 12);
            OLED_ShowNum(84, 44, E_Cnt, 3, 12);

            OLED_Refresh();
            DIO0_EnableInterrupt();
            HAL_TIM_Base_Start_IT(&TIM3_InitStruct);
        }
    }

    if(DIO0_GetState() == GPIO_PIN_SET)
    {
        uint8_t flag;
        SX127X_Read(REG_LR_IRQFLAGS, &flag);
        SX127X_Write(REG_LR_IRQFLAGS, 0xff); //clear flags
        if(flag & RFLR_IRQFLAGS_TXDONE)
        {
            communication_states = TX_DONE;
        }
        else if(flag & RFLR_IRQFLAGS_RXDONE)
        {
            communication_states = RX_DONE;
        }
    }
}
