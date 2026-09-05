# 🧠 DeepSexist: Detección Multimodal de Sexismo en Vídeo

Repositorio oficial del Trabajo de Fin de Máster (TFM) centrado en la detección automática de discursos de odio y misoginia en plataformas de vídeos cortos (ej. TikTok). Este proyecto ha sido desarrollado e inspirado bajo el marco de la competición internacional **EXIST 2025**.

## 🚀 Arquitectura del Sistema (Ensemble Multimodal)
Este proyecto supera los enfoques clásicos unimodales (basados solo en texto) proponiendo un **Ensemble Multimodal (Late Fusion)**. Debido a las limitaciones de VRAM (15 GB de la GPU T4 de Google Colab), la inferencia se realiza mediante un pipeline de **carga secuencial y liberación dinámica de memoria**, combinando los siguientes modelos:

1. **Audio/Texto:** Extracción con `MoviePY`, transcripción automática con `OpenAI Whisper-small`, y clasificación de odio empleando **Mistral-7B-Instruct-v0.3** (cuantizado a 4-bits con QLoRA).
2. **Visión Espacial (Imágenes):** Extracción de fotogramas estáticos con `OpenCV` y clasificación mediante **ConvNeXt** aplicando *Mean-Pooling*.
3. **Visión Temporal (Movimiento):** Procesado de secuencias de vídeo con **TimeSFormer** (16 frames) para captar dinámicas físicas como la cosificación.

## 📂 Estructura del Repositorio

* 📁 **`DatasetManagement/`**: Cuadernos de análisis exploratorio, partición estructurada del dataset (Task 3.1 y 3.3) y *scripts* automatizados de extracción de *frames* a partir de los vídeos originales.
* 📁 **`TrainingBooks/`**: Contiene los cuadernos de entrenamiento (*Fine-tuning*) para cada familia arquitectónica de forma aislada (Transformers clásicos, LLMs, ViT, TimeSFormer), los experimentos de Perspectivismo (LeWiDi) y el cálculo de pesos para el **Ensemble**.
* 📁 **`HuggingFaceSpace/`**: Incluye el cuaderno `DespliegueSpace.ipynb`, encargado de orquestar la inferencia secuencial de la aplicación final.

## 💻 ¿Cómo probar la Demo Interactiva?

Dado el inmenso tamaño de los modelos combinados, el despliegue estático en la nube no es viable de forma gratuita. El sistema funciona con una arquitectura Cliente-Servidor improvisada:

1. El backend se ejecuta en **Google Colab** ejecutando el cuaderno `HuggingFaceSpace/DespliegueSpace.ipynb`.
2. El cuaderno descarga y carga los modelos de forma secuencial en la GPU.
3. Se genera un túnel público mediante **Gradio** (`share=True`).
4. Dicho enlace se embebe en el archivo `index.html` de nuestro [Hugging Face Space público](https://huggingface.co/spaces/visumania2/DeepSexist-App), permitiendo usar la aplicación desde una web amigable sin que Hugging Face soporte la carga computacional.
