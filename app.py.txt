import streamlit as st
import google.generativeai as genai

# 1. Konfigurasi Halaman
st.set_page_config(page_title="ModulAjar AI", page_icon="📘")
st.title("📘 Pembuat Modul Ajar Otomatis")
st.write("Aplikasi ini membantu Guru membuat modul ajar dengan cepat.")

# 2. Bagian API Key (Agar aman, user memasukkan sendiri)
with st.sidebar:
    api_key = st.text_input("Masukkan API Key Gemini Anda:", type="password")
    st.info("Dapatkan API Key di Google AI Studio secara gratis.")

# 3. Logika Utama
if api_key:
    genai.configure(api_key=api_key)
    model = genai.GenerativeModel('gemini-1.5-flash')

    topik = st.text_input("Masukkan Topik Pelajaran:", placeholder="Contoh: Ekosistem Laut Kelas 5")
    
    if st.button("Buat Modul Sekarang"):
        if topik:
            with st.spinner('Sedang berpikir...'):
                prompt = f"Buatlah modul ajar lengkap untuk topik: {topik}. Sertakan tujuan pembelajaran, materi singkat, dan latihan soal."
                response = model.generate_content(prompt)
                st.markdown("---")
                st.markdown(response.text)
        else:
            st.warning("Isi topik dulu ya!")
else:
    st.warning("Silakan masukkan API Key di menu sebelah kiri untuk memulai.")