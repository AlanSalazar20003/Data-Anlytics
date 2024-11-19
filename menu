import streamlit as st
from streamlit_option_menu import option_menu
import pandas as pd
import plotly.express as px
import seaborn as sns
import matplotlib.pyplot as plt

# Load CSS file function
def load_css(file_name):
    with open(file_name) as f:
        st.markdown(f'<style>{f.read()}</style>', unsafe_allow_html=True)

# Cache the data loading function to avoid multiple reloading
@st.cache_data
def load_data():
    url = "https://raw.githubusercontent.com/AlanSalazar20003/Data-Anlytics/refs/heads/main/ifood_df.csv"
    return pd.read_csv(url)

# Load the data once
data = load_data()

# Load the CSS styles
load_css("style.css")

# Navigation Bar
with st.sidebar:
    selected = option_menu(
        menu_title=None,  # No main title
        options=["EDA", "Insights", "Filters"],
        icons=["bar-chart-line", "lightbulb", "filter"],
        menu_icon="cast",
        default_index=0,
        orientation="horizontal",
        styles={
            "container": {"padding": "0!important", "background-color": "#4A00E0"},
            "icon": {"color": "white", "font-size": "20px"},
            "nav-link": {"font-size": "18px", "color": "white", "text-align": "center"},
            "nav-link-selected": {"background-color": "white", "color": "#4A00E0"},
        },
    )

# Page Content
if selected == "EDA":
    st.subheader("Exploratory Data Analysis (EDA)")
    st.write("This is the EDA page.")

    # Plot 1: Total Spending by Education Level
    st.subheader("Total Spending by Education Level")
    fig, ax = plt.subplots(figsize=(10, 6))
    sns.barplot(data=data, x='education_2n Cycle', y='MntTotal', palette='Blues', ax=ax)
    ax.set_title("Total Spending by Education Level")
    ax.set_xlabel("Education Level")
    ax.set_ylabel("Total Spending")
    st.pyplot(fig)

    # Plot 2: Income by Marital Status
    st.subheader("Income by Marital Status")
    fig, ax = plt.subplots(figsize=(10, 6))
    sns.boxplot(data=data, x='marital_Married', y='Income', palette='Set2', ax=ax)
    ax.set_title("Income by Marital Status")
    ax.set_xlabel("Marital Status (Married)")
    ax.set_ylabel("Income")
    st.pyplot(fig)

    # Plot 3: Total Spending by Age
    st.subheader("Total Spending by Age")
    fig, ax = plt.subplots(figsize=(10, 6))
    sns.scatterplot(data=data, x='Age', y='MntTotal', hue='education_Graduation', palette='viridis', ax=ax)
    ax.set_title("Total Spending by Age")
    ax.set_xlabel("Age")
    ax.set_ylabel("Total Spending")
    st.pyplot(fig)

    # Plot 4: Distribution of Responses to Marketing Campaigns
    st.subheader("Distribution of Responses to Campaigns")
    fig, ax = plt.subplots(figsize=(8, 8))
    response_counts = data[['AcceptedCmp1', 'AcceptedCmp2', 'AcceptedCmp3', 'AcceptedCmp4', 'AcceptedCmp5']].sum()
    response_counts.plot.pie(autopct='%1.1f%%', colors=['#1f77b4', '#aec7e8', '#ff7f0e', '#ffbb78', '#2ca02c'], ax=ax)
    ax.set_title("Campaign Responses")
    ax.set_ylabel("")  # Hide the y-axis label
    st.pyplot(fig)

elif selected == "Insights":
    st.subheader("Insights")
    st.write("This is the Insights page.")

elif selected == "Filters":
    st.subheader("Filters")
    st.write("This is the Filters page.")

    # Map education levels to their corresponding one-hot encoded columns
    education_options = {
        "Basic": "education_Basic",
        "Graduation": "education_Graduation",
        "Master": "education_Master",
        "PhD": "education_PhD"
    }

    selected_education = st.selectbox("Select Education Level", list(education_options.keys()))
    education_column = education_options[selected_education]

    # Apply the filter based on the selected education level
    filtered_df = data[data[education_column] == 1]

    # Filter by range of 'Income' with a slider
    income_min = float(data['Income'].min())
    income_max = float(data['Income'].max())
    income_range = st.slider("Select Income Range", min_value=income_min, max_value=income_max, 
                              value=(income_min, income_max), step=0.1)
    filtered_df = filtered_df[(filtered_df['Income'] >= income_range[0]) & (filtered_df['Income'] <= income_range[1])]

    # Checkbox to show/hide a column 
    if st.checkbox("Show 'Recency' Column"):
        st.write("Recency column is displayed.")
    else:
        filtered_df = filtered_df.drop(columns=['Recency'])

    # Display the filtered dataset
    st.subheader("Filtered Food Shopping Data")
    st.write(filtered_df)

    # Distribution Plot for 'Income'
    st.subheader("Income Distribution")
    fig1 = px.histogram(data, x='Income', nbins=20, title='Distribution of Income')
    st.plotly_chart(fig1)
