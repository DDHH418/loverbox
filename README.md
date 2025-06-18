import streamlit as st
import random
from datetime import datetime
import os

st.set_page_config(page_title="LoverBox 💝", page_icon="💖", layout="centered")


page_bg = """
<style>
body {
    background: linear-gradient(135deg, #ffb6c1, #dda0dd);
    color: #4B0082;
    font-family: 'Segoe UI', sans-serif;
}
h1 {
    text-align: center;
    color: #FF1493;
}
.stButton > button {
    background-color: #d291bc;
    color: white;
    font-weight: bold;
    border-radius: 10px;
    border: none;
}
.stTextInput > div > input, .stTextArea textarea {
    border-radius: 5px; padding: 5px;
}
</style>
"""
st.markdown(page_bg, unsafe_allow_html=True)


if 'logged_in' not in st.session_state:
    st.session_state.logged_in = False
    st.session_state.user = ""

if not st.session_state.logged_in:
    st.title("🔐 Login to LoverBox")
    username = st.text_input("Username")
    password = st.text_input("Password", type="password")
    if st.button("Login"):
        if (username == "lover1" and password == "1234") or (username == "lover2" and password == "1234"):
            st.session_state.logged_in = True
            st.session_state.user = username
            st.experimental_rerun()
        else:
            st.error("Invalid credentials 💔")
    st.stop()


st.markdown(f"<h1>💖 LoverBox — Hello, {st.session_state.user}! 💖</h1>", unsafe_allow_html=True)


chat_file = f"chat_{st.session_state.user}.txt"


if os.path.exists(chat_file):
    with open(chat_file, "r", encoding="utf-8") as f:
        msgs = f.readlines()
else:
    msgs = []

st.subheader("💌 Private Chat")

for msg in reversed(msgs[-10:]):
    st.info(msg.strip())

name = st.session_state.user
msg_input = st.text_area("Your Message")
if st.button("Send Message"):
    if msg_input:
        with open(chat_file, "a", encoding="utf-8") as f:
            f.write(f"{name}: {msg_input}\n")
        st.experimental_rerun()
    else:
        st.warning("Write something first!")
