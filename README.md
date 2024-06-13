import csv
import pandas as pd
import streamlit as st

class DataHandler:
    def __init__(self, file_path):
        self.file_path = file_path
        self.data = None
    
    def load_data(self):
        try:
            self.data = pd.read_csv(self.file_path)
            return True
        except Exception as e:
            st.error(f"Error al cargar el archivo CSV: {e}")
            return False

    def preview_data(self):
        if self.data is not None:
            st.subheader("Primeras 10 filas del conjunto de datos:")
            st.write(self.data.head(10))
        else:
            st.error("Primero carga los datos antes de visualizarlos.")

    def calculate_statistics(self):
        if self.data is not None:
            st.subheader("Estadísticas descriptivas:")
            st.write(self.data.describe())
        else:
            st.error("Primero carga los datos antes de calcular estadísticas.")

def main():
    st.title("Aplicación para Análisis de Datos")

    st.sidebar.header("Cargar Datos")
    uploaded_file = st.sidebar.file_uploader("Cargar archivo CSV", type=["csv"])

    if uploaded_file is not None:
        data_handler = DataHandler(uploaded_file)
        if data_handler.load_data():
            st.success("Datos cargados correctamente.")
            data_handler.preview_data()
            data_handler.calculate_statistics()

if __name__ == "__main__":
    main()

