# seq_
import streamlit as st
import logomaker
import matplotlib.pyplot as plt
import pandas as pd
import io

st.title("DNA/RNA/Protein Sequence Logo Generator")

# Create a text area for input
sequences_input = st.text_area("Enter sequences (one per line)", height=200)

if st.button("Generate Logo"):
    if sequences_input:
        sequences = sequences_input.strip().splitlines()

        try:
            # Convert sequences into a pandas DataFrame for logomaker
            counts_df = logomaker.alignment_to_matrix(sequences, to_type='counts')

            # Generate the logo using Logomaker
            fig, ax = plt.subplots(figsize=(10, 3))
            logo = logomaker.Logo(counts_df, ax=ax)
            logo.style_spines(visible=False)
            logo.style_xticks(rotation=90)
            plt.tight_layout()

            # Show the logo on the Streamlit page
            st.pyplot(fig)

        except Exception as e:
            st.error(f"Error generating the logo: {e}")
    else:
        st.error("Please enter some sequences.")
