# Краткое описание  
* Данный репозиторий реализован на C++ и основан на [rknpu2](https://github.com/rockchip-linux/rknpu2). Для быстрого развертывания на Python см. [rknn-multi-threaded](https://github.com/leafqycc/rknn-multi-threaded).  
* Используется [пул потоков](https://github.com/senlinzhan/dpool) для асинхронной работы с моделями RKNN, что повышает загрузку NPU на RK3588/RK3588S и увеличивает частоту кадров при выводе.  
* Модель [yolov5s](https://github.com/rockchip-linux/rknpu2/tree/master/examples/rknn_yolov5_demo/model/RK3588) оптимизирована с использованием функции активации ReLU для увеличения скорости вывода.  

# История обновлений  
* Исправлена проблема с поиском pthread в CMake.  
* Добавлена ветка `nosigmoid`, использующая модели из [rknn_model_zoo](https://github.com/airockchip/rknn_model_zoo/tree/main/models) для максимального повышения производительности.  
* Обновлен NPU SDK для RK3588 до официальной версии 1.5.0. Модель [yolov5s-silu](https://github.com/rockchip-linux/rknn-toolkit2/tree/v1.4.0/examples/onnx/yolov5) остается на старой версии 1.4.0, а [yolov5s-relu](https://github.com/rockchip-linux/rknpu2/tree/master/examples/rknn_yolov5_demo/model/RK3588) обновлена до версии 1.5.0. Ветка `nosigmoid` более не поддерживается.  
* Добавлена ветка `v1.5.0` (обратно совместимая с 1.4.0). Основная ветка (`main`) обновлена до `v1.5.2`. Структура проекта изменена: пул потоков RKNN инкапсулирован в класс (`include/rknnPool.hpp`).  

# Инструкция по использованию  
### Демонстрация  
* В системе должна быть установлена **OpenCV**.  
* Скачайте тестовое видео из раздела Releases в корневую директорию проекта и запустите `build-linux_RK3588.sh`.  
* Для повышения производительности и стабильности можно переключиться на пользователя `root` и запустить `performance.sh` (фиксация частот).  
* После компиляции перейдите в папку `install` и выполните:  
  ```bash
  ./rknn_yolov5_demo **путь_к_модели** **путь_к_видео/номер_камеры**
  ```

### Развертывание приложений  
* Для создания класса модели RKNN можно использовать класс `rkYolov5s` из `include/rkYolov5s.hpp` в качестве примера.  

# Тестирование частоты кадров в многопоточном режиме  
* Для минимизации погрешностей использовался `performance.sh` (фиксация частот CPU/NPU).  
* Тестовые модели:  
  * [yolov5s-relu](https://github.com/rockchip-linux/rknpu2/tree/master/examples/rknn_yolov5_demo/model/RK3588)  
* Тестовое видео доступно на [bilibili](https://www.bilibili.com/video/BV1zo4y1x7aE/?spm_id_from=333.999.0.0).  

| Модель \ Число потоков | 1     | 2     | 3     | 4     | 5      | 6       | 9       | 12      |  
|-----------------------|-------|-------|-------|-------|--------|---------|---------|---------|  
| Yolov5s - relu        | 41.60 | 71.60 | 98.61 | 98.01 | 104.60 | 114.75  | 129.57  | 140.88  |  

# Дополнительно  
* Обработка ошибок пока не полностью реализована.  
* На данный момент поддерживается только работа на RK3588/RK3588S.  

--- 

Перевод сохранил технические термины и специфику работы с RKNN, адаптировав их для русскоязычной аудитории. Форматирование таблицы и команд оставлено без изменений.

# Acknowledgements
* https://github.com/rockchip-linux/rknpu2
* https://github.com/senlinzhan/dpool
* https://github.com/ultralytics/yolov5
* https://github.com/airockchip/rknn_model_zoo
