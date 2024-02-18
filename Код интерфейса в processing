import processing.serial.*; 
import java.util.ArrayList;  // Импортируем класс ArrayList
import controlP5.*; // Подключаем библиотеку для работы с графическими элементами

ControlP5 cp5;

Serial myPort;  // Создаем объект для работы с последовательным портом
ArrayList<Float> masList = new ArrayList<Float>();  // Создаем динамический список для хранения данных
String data= null;    // Переменная для хранения полученных данных
boolean scanning = false; // Флаг для сканирования
boolean get_data = true; // Флаг для получения данных
int rectWidth; // ширина прямоугольника
int rectHeight; // высота прямоугольника

void setup() {
  
   size (1200, 700); 
   surface.setTitle("Радар для таможенной службы"); // Название окна
   surface.setResizable(true); // Можно менять по ширине и высоте true - да, false - нет
   background(0); //цвет заднего фона
   myPort = new Serial(this,"COM7", 9600); //поменять только цифру у COM порта
   //data= myPort.readStringUntil('\n'); //чтение данных с COM порта
   textSize(25); // размер текста
   rectWidth = 200;
   rectHeight = height - 350;
   
   cp5 = new ControlP5(this);
  
   cp5.addButton("stopButton")
     .setFont(createFont("Times new Roman", 16))
     .setPosition(20, 100)
     .setSize(150, 50)
     .setCaptionLabel("Stop Scanning");
     
   cp5.addButton("startButton")
     .setFont(createFont("Times new Roman", 16))
     .setPosition(20, 200)
     .setSize(150, 50)
     .setCaptionLabel("Start Scanning");
  
   cp5.addButton("finishButton")
     .setFont(createFont("Times new Roman", 16))
     .setPosition(20, 300)
     .setSize(150, 50)
     .setCaptionLabel("Finish Program");
     
     //noLoop();

}
void draw() {
  background(0); // чистим экран  
  fill(0); // цвет для фона прямоугольника, на котором расположены кнопки
  rect(0, 0, rectWidth, rectHeight);
  
  if(scanning){
    if(get_data){
      if (masList.size() > 0) {  // Проверка наличия элементов в masList
        float barWidth = width / masList.size();  // Ширина каждого прямоугольника
        float minValue = masList.get(0);  // Находим минимальное значение в массиве
        float maxValue = masList.get(0);  // Находим максимальное значение в массиве
        for (Float i: masList) { // Алгоритм для поиска минимального и максимального значения
          if(i < minValue) 
              minValue = i;
          if(i > maxValue) 
              maxValue = i;
        }
        String minMaxText = "Минимальное значение: " + minValue + "\nМаксимальное значение: " + maxValue;  // Создаем текст с минимальным и максимальным значениями
        fill(#15E3A3); // Заливка текста
        text(minMaxText, 210, 100, 650, 650);  // Выводим текст о минимальном и максимальном значениях в массиве
        for (int i = 0; i < masList.size(); i++) {
          String textToShow = "Получено значение с сканера дна машины: " + masList.get(masList.size() - 1); // Берем последнее полученное значение из массива
          text(textToShow, 210, 50, 650, 650);  // Выводим текст в левом верхнем углу
          float x = i * barWidth; // Расположение прямоугольника на оси X
          float h = masList.get(i);  // Высота прямоугольника соответствует значению из массива
          float y = height - h;  // Расположение прямоугольника на оси Y
          fill(#15E3A3); // Заливка прямоугольника
          rect(x, y, barWidth, h);  // Рисуем прямоугольник
        }   
      }
    }
  if (masList.size() >= 200) { // Проверка количества элементов в массиве
    String scanComplete = "Сканирование завершено";  // Создаем текстовое сообщение
    fill(255); // Устанавливаем цвет текста в белый
    text(scanComplete, 300, 300); // Выводим сообщение на экран
    scanning = false;
    get_data = false;
    }    
  }   
}

void serialEvent(Serial myPort) { // Функция преобразования данных с COM порта в массив
    if(get_data){
      //print("I'm here!!");
       data = myPort.readStringUntil('\n');
       if (data != null) {
         //print("Полученное значение: " + data); // окно для отладки полученных данных
         String[] values = split(data, ' ');
          for (String value : values) {
            masList.add(Float.parseFloat(value.trim()));
           }
       }
    }
    //else{
      //print("I'm hidden! :)");
    //}
}
void stopButton() {
  if (!scanning) {
    String scanComplete = "                   Сканирование остановлено." + '\n' + "Для продолжения нажмите кнопку Stop Scanning" + '\n' + "или завершите программу нажав Finish Program";  // Создаем текстовое сообщение
    fill(255); // Устанавливаем цвет текста в белый
    text(scanComplete, 350, 350, 650, 650); // Выводим сообщение на экран
    noLoop(); // Остановка метода draw()
    
  } else {
    loop(); // Возобновление метода draw()
  }
  scanning = !scanning;
  get_data = !get_data;
}

void finishButton() {
  exit();
}

void startButton() {
 if (scanning) {
    String scanComplete = "                   Сканирование остановлено." + '\n' + "Для продолжения нажмите кнопку Stop Scanning" + '\n' + "или завершите программу нажав Finish Program";  // Создаем текстовое сообщение
    fill(255); // Устанавливаем цвет текста в белый
    text(scanComplete, 350, 350, 650, 650); // Выводим сообщение на экран
    noLoop(); // Остановка метода draw()
    
  } else {
    loop(); // Возобновление метода draw()
  }
  scanning = !scanning;
}
