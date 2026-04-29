import streamlit as st
import requests
import pandas as pd

st.set_page_config(page_title="AINU.SYSTEMS - Narayama", layout="wide")

# URL da API
API_URL = "https://backend-ainu-systems.onrender.com"

st.title("🌍 AINU.SYSTEMS - Narayama v2.1")
st.write("**27 Países | Índices Globais | Análise Sistemática**")
st.divider()

# Tabs
tab1, tab2, tab3, tab4 = st.tabs(["📊 Países", "🏆 Ranking", "🌐 Regiões", "📈 Status"])

# ============================================================================
# TAB 1: PAÍSES
# ============================================================================
with tab1:
    st.header("📊 Lista de 27 Países")
    
    try:
        response = requests.get(f"{API_URL}/paises")
        if response.status_code == 200:
            data = response.json()
            paises = data.get("paises", [])
            
            # Criar DataFrame
            df = pd.DataFrame(paises)
            df = df[["codigo", "nome", "n_index"]].rename(columns={
                "codigo": "Código",
                "nome": "País",
                "n_index": "Índice N"
            })
            
            st.dataframe(df, use_container_width=True, height=600)
            
            # Estatísticas
            col1, col2, col3, col4 = st.columns(4)
            with col1:
                st.metric("Total de Países", len(paises))
            with col2:
                st.metric("Índice Máximo", f"{max([p['n_index'] for p in paises]):.2f}")
            with col3:
                st.metric("Índice Mínimo", f"{min([p['n_index'] for p in paises]):.2f}")
            with col4:
                st.metric("Índice Médio", f"{sum([p['n_index'] for p in paises])/len(paises):.2f}")
        else:
            st.error("❌ Erro ao conectar à API")
    except Exception as e:
        st.error(f"❌ Erro: {str(e)}")

# ============================================================================
# TAB 2: RANKING
# ============================================================================
with tab2:
    st.header("🏆 Ranking de Países")
    
    try:
        response = requests.get(f"{API_URL}/ranking")
        if response.status_code == 200:
            data = response.json()
            ranking = data.get("ranking", [])
            
            # Criar DataFrame com posição
            df_ranking = pd.DataFrame(ranking)
            df_ranking.insert(0, "Posição", range(1, len(df_ranking) + 1))
            df_ranking = df_ranking[["Posição", "codigo", "nome", "n_index"]].rename(columns={
                "codigo": "Código",
                "nome": "País",
                "n_index": "Índice N"
            })
            
            st.dataframe(df_ranking, use_container_width=True, height=600)
            
            # Top 5
            st.subheader("🥇 Top 5")
            cols = st.columns(5)
            for i, pais in enumerate(ranking[:5]):
                with cols[i]:
                    st.metric(f"#{i+1}", pais["nome"], f"{pais['n_index']:.2f}")
        else:
            st.error("❌ Erro ao conectar à API")
    except Exception as e:
        st.error(f"❌ Erro: {str(e)}")

# ============================================================================
# TAB 3: REGIÕES
# ============================================================================
with tab3:
    st.header("🌐 Países por Região")
    
    try:
        response = requests.get(f"{API_URL}/regioes")
        if response.status_code == 200:
            data = response.json()
            regioes = data.get("regioes", {})
            
            # Exibir por região
            for regiao, paises in regioes.items():
                if paises:  # Só mostra se tiver países
                    with st.expander(f"📍 {regiao} ({len(paises)} países)"):
                        df_regiao = pd.DataFrame(paises)
                        df_regiao = df_regiao[["codigo", "nome", "n_index"]].rename(columns={
                            "codigo": "Código",
                            "nome": "País",
                            "n_index": "Índice N"
                        })
                        st.dataframe(df_regiao, use_container_width=True)
        else:
            st.error("❌ Erro ao conectar à API")
    except Exception as e:
        st.error(f"❌ Erro: {str(e)}")

# ============================================================================
# TAB 4: STATUS
# ============================================================================
with tab4:
    st.header("📈 Status do Sistema")
    
    try:
        response = requests.get(f"{API_URL}/status")
        if response.status_code == 200:
            data = response.json()
            
            col1, col2, col3 = st.columns(3)
            with col1:
                st.metric("Status", "🟢 Online")
            with col2:
                st.metric("Versão", data.get("versao", "N/A"))
            with col3:
                st.metric("Países Ativos", data.get("paises_ativos", 0))
            
            st.info(f"✅ API respondendo normalmente\n📊 Timestamp: {data.get('timestamp', 'N/A')}")
        else:
            st.error("❌ API offline")
    except Exception as e:
        st.error(f"❌ Erro ao conectar: {str(e)}")

# ============================================================================
# FOOTER
# ============================================================================
st.divider()
st.markdown("""
---
**AINU.SYSTEMS v2.1** | Índices Narayama | 27 Países Globais
""")
