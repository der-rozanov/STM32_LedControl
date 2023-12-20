В начале изучения нового языка программирования традиционно выводят фразу Hello World, в начале изчуения нового контроллера традиционно моргают светодиолом встроенным в плату. Вот и я, начиная свою работу с STM32 Nucleo не стал отходить от традиций. 

     HAL_GPIO_WritePin(GPIOA, LD2_Pin, HIGH); //даем высокий сигнал на выход светодиодла

Но это слышком скучно, потому добавим управление с кнопки и вывод в COM Port. 


    unsigned char onMsg[] = "Button is pressed, LED is on\r\n"; //сообщение
    unsigned char offMsg[] = "Button released, Led is off\r\n"; //сообщение
    _Bool buttonCondition = 1;
    while(1)
    {
    if(HAL_GPIO_ReadPin(GPIOC, B1_Pin) == 0 && buttonCondition == 1) //Условие того что состояние кнопки сменилось
      {
    	  HAL_GPIO_WritePin(GPIOA, LD2_Pin, HIGH); //даем высокий сигнал на выход светодиодла
    	  HAL_UART_Transmit(&huart2,onMsg,sizeof(onMsg),10); //пускаем в ком порт сообщение
    	  buttonCondition = 0; //запоминаем новое состояние кнопки
      }
      if(HAL_GPIO_ReadPin(GPIOC, B1_Pin) == 1 && buttonCondition == 0) //аналогично
      {
          HAL_GPIO_WritePin(GPIOA, LD2_Pin, LOW);
          HAL_UART_Transmit(&huart2,offMsg,sizeof(offMsg),10);
          buttonCondition = 1;
      }
    }

Запоминая состояние кнопки мы игнорируем повторяющиеся случаи и не выполняем избыточные команды, включая уже включенный светодиод и отправляя одно и то же сообщение. 
