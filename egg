import streamlit as st
import json

DATA_FILE = "chatbot_data.json"

def load_data():
    try:
        with open(DATA_FILE, 'r') as f:
            return json.load(f)
    except FileNotFoundError:
        return []

def save_data(data):
    with open(DATA_FILE, 'w') as f:
        json.dump(data, f, indent=4)

def suggest_context(current_question, data):
    if len(current_question) > 2:
        all_contexts = [item['context'].lower() for item in data]
        unique_contexts = sorted(list(set(all_contexts)))
        suggestions = [
            ctx for ctx in unique_contexts if current_question.lower() in ctx
        ]
        return suggestions[:5]  # Limit to a few suggestions
    return []

def main():
    st.title("Chatbot Data Recorder")

    data = load_data()

    question = st.text_input("Question:")
    answer = st.text_area("Answer:")

    # Context input with suggestions
    context = st.text_input("Context:", key="context_input")
    if question:
        suggested_contexts = suggest_context(question, data)
        if suggested_contexts:
            st.info("Suggested Contexts:")
            for suggestion in suggested_contexts:
                if st.button(f"Use: {suggestion}", key=f"suggestion_{suggestion}"):
                    st.session_state.context_input = suggestion
                    st.rerun()  # Rerun to update the context input field

    if st.button("Save Data"):
        if question and answer and st.session_state.context_input:
            data.append({"question": question, "answer": answer, "context": st.session_state.context_input})
            save_data(data)
            st.success("Data saved successfully!")
        else:
            st.error("Please fill in all fields.")

if __name__ == "__main__":
    main()
