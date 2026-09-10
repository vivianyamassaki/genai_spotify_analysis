# 🎧 Análise de dados do Spotify com Gemini

Pipeline analítico e comportamental em Python para explorar o **Histórico Estendido de Streaming do Spotify**, combinando análise de dados e storytelling visual com GenAI. 

O projeto vai além da retrospectiva anual padrão, tratando logs brutos, corrigindo fusos horários e aplicando modelagens de rotina, retenção e fadiga sonora. O relatório gerado pelo notebook foi fornecido para o Gemini para que ele gerasse infográficos personalizados com as análises feitas.

---

## 📌 Destaques do Projeto

- **Engenharia de Features:** Conversão de fusos horários (UTC para horário local), categorização de janelas horárias e métricas de sessões válidas (> 30s).
- **Métricas Comportamentais:**
  - **Índice de Exploração vs. Nostalgia:** Balanço quantitativo entre descoberta de novos sons e repetição.
  - **Taxa de Rejeição (Skip Rate):** Mensuração de pulos intencionais (`fwdbtn`) por artista.
  - **Vício Sequencial & Streaks:** Mapeamento de faixas colocadas no *repeat* e dias consecutivos ouvindo o mesmo artista.
  - **Raio-X de Artista:** Análise de amplitude de catálogo versus repetição de hits.
- **Modelagem Avançada:** análise de sobrevivência de faixas (tempo da descoberta até a fadiga).
- **Visualização & Storytelling:** Scripts para *Bar Chart Race* animado e template HTML com estética minimalista inspirada no Notion.

---

## 📊 Métricas Chave

$$\text{Índice de Exploração (\%)} = \left(\frac{\text{Músicas Únicas}}{\text{Total de Plays}}\right) \times 100$$

- **Próximo a 100%:** Perfil explorador, alta taxa de novidades e diversidade no catálogo.
- **Valores menores:** Perfil apegado à zona de conforto, playlists recorrentes e modo *repeat*.

---

## 🚀 Como Executar

### Opção 1: Google Colab (Recomendado)

1. Acesse o [Google Colab](https://colab.research.google.com/).
2. Vá na aba **GitHub**, insira a URL do repositório:
   ```text
   [https://github.com/vivianyamassaki/genai_spotify_analysis](https://github.com/vivianyamassaki/genai_spotify_analysis)
   ```
3. Selecione o notebook do projeto para abrir.
4. Execute as células sequencialmente:
   - **Com dados simulados:** Execute diretamente (o gerador de mock data assume o fluxo automaticamente caso nenhum arquivo seja carregado).
   - **Com seus dados reais:** Faça o upload dos arquivos `.json` do seu Histórico Estendido do Spotify pelo painel lateral de arquivos do Colab e aponte o caminho no código.

---

### Opção 2: Localmente (Jupyter / VS Code)

1. Clone o repositório:
   ```bash
   git clone [https://github.com/vivianyamassaki/genai_spotify_analysis.git](https://github.com/vivianyamassaki/genai_spotify_analysis.git)
   cd genai_spotify_analysis
   ```

2. Crie e ative um ambiente virtual:
   ```bash
   python -m venv .venv
   source .venv/bin/activate  # Windows: .venv\Scripts\activate
   ```

3. Instale as dependências necessárias:
   ```bash
   pip install pandas numpy matplotlib seaborn plotly scikit-learn
   ```

4. Inicie o Jupyter:
   ```bash
   jupyter notebook
   ```

---

## 🗂️ Arquivos Suportados (Dados Reais)

Caso queira usar seus próprios dados, solicite o **Histórico Estendido de Streaming** nas configurações de privacidade do Spotify:

| Arquivo | Descrição |
| :--- | :--- |
| `Streaming_History_Audio_*.json` | Logs completos de reprodução (`ts`, `ms_played`, artistas, faixas, motivos de início/fim e skips). |
| `YourLibrary.json` + `AddedToCollection.json` | Cruzamento de eventos de *like* e faixas salvas com a biblioteca legível. |
| `Playlist*.json` / `AddedToPlaylist.json` | Dados de criação e curadoria de playlists. |
| `SearchViewResponse.json` | Histórico de buscas ativas dentro do app. |

---

## 🎨 Saída Visual (Infográfico com base na saída do relatório)
Com o relatório gerado pelo notebook, você pode criar uma [Gem](https://gemini.google.com/gems) para criar um infográfico personalizado com as análises feitas.
Abaixo, deixo de sugestão o que fiz como teste inicial deste projeto.

**Nome:** Gerador de Infográfico Spotify (Notion Style)

**Descrição:** Assistente focado em design web e storytelling de dados. Recebe métricas mensais extraídas do Spotify e gera um infográfico responsivo em arquivo HTML único, com identidade visual minimalista (tons pastéis e cards).

**Instruções:**
```
# PAPEL E OBJETIVO
Você é um desenvolvedor front-end e especialista em visualização de dados. Seu objetivo é receber métricas de escuta mensais do Spotify e gerar um código HTML/CSS completo, responsivo e autocontido (standalone) no formato de um infográfico minimalista.

# DIRETRIZES VISUAIS (NOTION STYLE)
- **Cores:** Fundo bege suave (`#F7F6F3`), cards brancos com sombras sutis (`box-shadow: 0 8px 24px rgba(0, 0, 0, 0.04)`) e bordas suaves (`#E9E9E7`).
- **Acentos (Pastéis):** Verde (`#73C79D`), Roxo (`#C2A3D1`), Coral (`#F5A384`), Laranja (`#E8A858`), Azul (`#94C1EB`).
- **Tipografia:** Fonte `Nunito` do Google Fonts.
- **Ícones:** FontAwesome (via CDN).

# ESTRUTURA ESPERADA DE ENTRADA
O usuário fornecerá os seguintes dados agregados (geralmente extraídos via Pandas/Python):
1. Mês/Ano.
2. Overview: Minutos Ouvidos, Faixas Iniciadas, Artistas Distintos, Índice de Exploração.
3. Top 5 Artistas (com minutos tocados).
4. Top 5 Faixas (com número de plays).
5. DNA das Mais Ouvidas (Título, 3 parágrafos de análise de storytelling e 4 tags).
6. Hábitos Diários: Média diária (minutos), Dia mais musical (dia e minutos), Janela mais ativa (Turno e faixa de horário).
7. Maratonas/Streaks: Maior sessão (horas e faixas contínuas), Artista em alta (dias de streak), Data da sessão.

# REGRAS DE SAÍDA (O TEMPLATE)
- O código gerado deve ser EXATAMENTE um arquivo HTML válido, começando com `<!DOCTYPE html>`.
- Não abrevie o CSS. Inclua todo o estilo necessário para que o grid funcione perfeitamente.
- O bloco de "Overview Stats" deve ter 4 colunas em telas grandes (`grid-template-columns: repeat(4, 1fr)`).
- O bloco de "Top Artistas" e "Top Faixas" deve usar barras de preenchimento relativas (`item-bar-green` e `item-bar-purple`) baseadas nos valores fornecidos, com o CSS `width: X%` calculado dinamicamente ou inserido inline.
- A explicação do "Índice de Exploração" deve ficar sempre posicionada no final do infográfico, antes do rodapé.
- Substitua todos os valores fictícios do template pelos dados reais fornecidos pelo usuário.

Entregue APENAS o bloco de código HTML/CSS, sem explicações adicionais, para que o usuário possa copiar e salvar diretamente.
```
