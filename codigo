import streamlit as st
import openai

# Interface Streamlit
st.title('Criticador e Revisor de User Stories')

# Entrada da chave da API
api_key = st.text_input("Insira sua chave API da OpenAI aqui:", type="password")

context = st.text_area("Insira o contexto aqui (opcional):", height=150)
user_story = st.text_area("Insira a User Story aqui:", height=150)
role = st.selectbox('Selecione o papel para a crítica:', ['Desenvolvedor', 'QA', 'Negócios'])

# Função para criticar a user story
def critique_user_story(api_key, context, story, role):
    if context:
        prompt_content = f"Contexto: {context}\nComo {role.lower()}, critique a seguinte user story: {story}"
    else:
        prompt_content = f"Como {role.lower()}, critique a seguinte user story: {story}"

    messages = [
        {"role": "system", "content": "Você é um assistente experiente."},
        {"role": "user", "content": prompt_content}
    ]

    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=messages,
        max_tokens=512,
        api_key=api_key  # Usando a chave fornecida no frontend
    )
    return response['choices'][0]['message']['content']

# Função para reescrever a user story
def rewrite_user_story(api_key, context, story):
    if context:
        prompt_content = f"Com base no seguinte contexto: {context}, reescreva esta user story para ser mais clara e eficaz: {story}"
    else:
        prompt_content = f"Reescreva esta user story para ser mais clara e eficaz: {story}"

    messages = [
        {"role": "system", "content": "Você é um assistente de redação."},
        {"role": "user", "content": prompt_content}
    ]

    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=messages,
        max_tokens=512,
        api_key=api_key  # Usando a chave fornecida no frontend
    )
    return response['choices'][0]['message']['content']

if api_key:
    if st.button('Criticar User Story'):
        if user_story:
            feedback = critique_user_story(api_key, context, user_story, role)
            st.subheader('Feedback da Crítica:')
            st.write(feedback)
        else:
            st.error("Por favor, insira uma user story antes de prosseguir.")

    if st.button('Reescrever User Story'):
        if user_story:
            new_story = rewrite_user_story(api_key, context, user_story)
            st.subheader('User Story Reescrita:')
            st.write(new_story)
        else:
            st.error("Por favor, insira uma user story antes de prosseguir.")
else:
    st.error("Por favor, insira sua chave API da OpenAI para continuar.")
