# AssetLad-Dashboard
import streamlit as st
import yfinance as yf
import pandas as pd

st.title('@TheAssetLad Dashboard 🌍')

ticker = ('MSFT','META', 'PLTR','BTC-USD','ETH-USD')

dropdown = st.multiselect('Pick your Assets',
                          ticker)

start = st.date_input('Start', value = pd.to_datetime('2021-01-01'))
end = st.date_input('End', value = pd.to_datetime('today'))

def relativeret(df):
    rel = df.pct_change()
    cumret = (1+rel).cumprod() - 1 
    cumret =cumret.fillna(0) 
    return cumret


if len(dropdown) > 0:
    #df = yf.download(dropdown,start,end)['Adj Close']
    
    df = relativeret(yf.download(dropdown,start,end)['Adj Close'])
    st.header('Returns of {}'.format(dropdown))
    
    st.line_chart(df)
    
