# PNEUMONIA_DETECTION
Predicting whether a person is infected with pneumonia or not. The model was trained using VGG19 CNN Architecture.
And Was deployed on to web using Streamlit and Pyngrok
import streamlit as st
import tensorflow as tf
from keras.models import load_model

st.set_option('deprecation.showfileUploaderEncoding',False)
@st.cache(allow_output_mutation=True)
def load_classfication_model():
  model = load_model('/content/model_pneumonia_detection.h5')
  return model

model = load_classfication_model()

st.write("""
          # PNEUMONIA DETECTION USING CNN
          """)

file = st.file_uploader("Please upload a Chest XRAY IMAGE",type = ["jpeg","jpg","png"])

import cv2
from PIL import Image, ImageOps
import numpy as np

def import_and_predict(image_data,model):

  size = (224,224)
  image = ImageOps.fit(image_data,size,Image.ANTIALIAS)
  img = np.asarray(image)
  img_reshape = img[np.newaxis,...]
  prediction = model.predict(img_reshape)

  return prediction

if file is None:
  st.text("Please upload an image file")
else:
  image = Image.open(file)
  st.image(image,use_column_width=True)
  predictions = import_and_predict(image,model)
  class_names = ['Normal','Pneumonia']
  string = "This particular image most likely is :"+class_names[np.argmax(predictions)]
  st.success(string)
