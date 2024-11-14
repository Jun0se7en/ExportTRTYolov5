# ExportTRTYolov5

## Description

This repo will help you export Yolov5 model to TensorRT on Jetson board

## Export Steps

1. **Clone the Repository**

    ```bash
    git clone https://github.com/Jun0se7en/ExportTRTYolov5.git
    cd ExportTRTYolov5
    ```

2. **Export Model**

    ```bash
    # Generate WTS file change yolov5s to your model name
    python3 gen_wts.py -w yolov5s.pt -o yolov5s.wts
    # Cmake & Make 
    # If using custom model, make sure to update kNumClas, kInputH, kInputW to what you have trained in yolov5/src/config.h
    cd yolov5/
    mkdir build
    cd build
    # Copy your model you have exported to wts above to build directory
    cp ../../yolov5s.wts .
    cmake ..
    make
    # Build engine, remember to change the letter at last that stand for nano (n), large (l), small (s), medium (m) of the model you have trained
    ./yolov5_det -s yolov5s.wts yolov5s.engine s
    ```

3. **Inference**

    ```bash
    # Import YoloTRT from yoloDet.py included in this repo
    from yoloDet import YoloTRT
    import cv2
    # Directory to model
    engine = 'yolov5n.engine'
    # Conf thres for each class
    conf_thres = 0.6
    # IoU thres for bounding box of each class
    iou_thres = 0.4
    # Change this classes based on your model
    classes = ["person", "bicycle", "car", "motorcycle", "airplane", "bus", "train", "truck", "boat", "traffic light",
            "fire hydrant", "stop sign", "parking meter", "bench", "bird", "cat", "dog", "horse", "sheep", "cow",
            "elephant", "bear", "zebra", "giraffe", "backpack", "umbrella", "handbag", "tie", "suitcase", "frisbee",
            "skis", "snowboard", "sports ball", "kite", "baseball bat", "baseball glove", "skateboard", "surfboard",
            "tennis racket", "bottle", "wine glass", "cup", "fork", "knife", "spoon", "bowl", "banana", "apple",
            "sandwich", "orange", "broccoli", "carrot", "hot dog", "pizza", "donut", "cake", "chair", "couch",
            "potted plant", "bed", "dining table", "toilet", "tv", "laptop", "mouse", "remote", "keyboard", "cell phone",
            "microwave", "oven", "toaster", "sink", "refrigerator", "book", "clock", "vase", "scissors", "teddy bear",
    model = YoloTRT(engine=engine, conf=conf_thres, categories=classes, iou=iou_thres, debugging=True)
    image = cv2.imread('test.img')
    inf_image = image.copy()
    # res will contain class, bb; t will be inferenced time; Your input image will have bb drawn on it so use copy before inference
    res, t = model.infer(inf_image)
    ```

## Pretrained Model

[Pretrained](https://drive.google.com/file/d/1Cg-jAYYiG2T1N0tlNaRi9KnGBbo0oEJm/view?usp=sharing)

## License

This project is licensed under the MIT License.

## References

- [JetsonYolov5](https://github.com/mailrocketsystems/JetsonYolov5)
