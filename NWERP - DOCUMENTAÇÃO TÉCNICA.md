

Este documento apresenta uma visão geral técnica da NWERP, plataforma complementar ao ERP legado, projetada para integrar dados em tempo real, oferecer automação inteligente, processos auditáveis com BPMN e interfaces modernas em Flutter e React.

---

## 🛠️ Tecnologias

### Backend
- **C# .NET 8** (núcleo dos serviços)
- **Entity Framework e Dapper** (persistência)
- **Hangfire** (jobs agendados e recorrentes)
- **Kafka** (mensageria assíncrona)
- **Debezium** (captura de mudanças - CDC)

### Orquestração de Processos
- **Camunda BPMN**

### Frontend
- **Flutter** (mobile)
- **Fork Camunda Tasklist** (web)

### Observabilidade e Segurança
- **OpenTelemetry** (logs estruturados e rastreáveis)
- **JWT com Supabase Auth** (autenticação)

### Bases de Dados
- **PostgreSQL**
- **Supabase** (dados e autenticação)

---

## 📐 Arquitetura Geral


```mermaid
block-beta
	columns 3
	block:e:3
		a["data.core"]
		b("insigth.ai")
		c["operador+"]
		d["flow.human"]		
	end

```




```mermaid
flowchart TD
	ERP1[(ERP Legado)]
    ERP1 -->|Debezium CDC| Kafka[Event Stream - Kafka]
    ERP1 -->|Query-Based C#| Kafka[Event Stream - Kafka]
    Kafka --> Worker1[data.core - .NET 8]
    Worker1 --> DB[(PostgreSQL + Supabase)]
    DB --> API[(API REST)]
	API --> FORMS[Flutter & Tasklist Web]
	API --> UI[Dashboards & Insights+]
	API --> BMPNS[Sistema de BPMN]	 
    API --> |Integrações| ERP1
```







---

## 🔄 data.core

* Streaming de dados do ERP
* Importação/Integração de dados externos para o ERP
* Exportação de Logs

```mermaid
sequenceDiagram
    participant ERP as ERP Legado
    participant Deb as Debezium
    participant Kafka as Kafka
    participant Core as Worker .NET
    participant PG as PostgreSQL

    ERP-->>Deb: Alteração detectada
    Deb-->>Kafka: Evento enviado
    Kafka-->>Core: Evento recebido
    Core-->>PG: Dados processados e armazenados
```


---

## 🧠 insight.ai
- Análise e consulta inteligente
- Geração automática de gráficos e dashboards
- Interações via Flutter/React (UI)

---

## 🤖 operador+
- Agente digital com LLM
- Executa comandos via eventos ou linguagem natural
- Comunicação via APIs REST
- Interface chatbot e Flutter 

```mermaid
	sequenceDiagram
	    participant User as Usuário
	    participant Flutter as Interface Flutter
	    participant API as API REST NWERP
	    participant Operador as operador+
	    participant LLM as LLM (OpenAI, etc.)
	    participant BPMN as BPMNS
	
	    User->>Flutter: Envia requisição (comando ou pergunta)
	    Flutter->>API: Requisição via API REST
	    API->>Operador: Recebe e processa requisição
	    Operador->>LLM: Consulta Inteligente (IA)
	    LLM-->>Operador: Retorna resposta ou ação recomendada
	    Operador->>BPMN: Dispara processo BPMN (opcional)
	    Operador-->>API: Resposta estruturada
	    API-->>Flutter: Retorno via API REST
	    Flutter-->>User: Exibe resposta ou confirmação da ação
```

---

## 👤 flow.human 

- Processos BPMN com Camunda
- Tarefas automáticas e humanas
- Interface via Flutter e Tasklist Web
- Execução rastreável e auditável

```mermaid
graph TD
    Start([Início]) --> Auto[Etapa Automática]
    Auto --> Human[Etapa Humana] --> End([Fim])
```

---

- **Flutter** (iOS, Android)
- **Fork do Camunda Tasklist** (interfaces web customizadas)

**Benefícios:**
- Interface consistente e otimizada
- Desenvolvimento acelerado com fork
- Alta flexibilidade

**Cuidados:**
- Manutenção contínua do fork
- Sincronização periódica com atualizações Camunda

---

## 🚀 CI/CD e Deploy
- Containers Docker
- GitHub Actions e GitLab CI
- Versionamento Semântico
- Ambientes: desenvolvimento, staging, produção

---

## 📅 Roadmap
- Novos conectores (SAP, Protheus)
- Regras via DMN
- Decisões automatizadas (`operador+`)
- SDK para Flutter (tarefas customizadas)

---

## 📩 Contato Técnico
- **Email:** dev@nwerp.com.br
- **Git:** Privado (SSH)

---

## 🚧 Considerações Finais

A arquitetura NWERP promove flexibilidade e robustez, permitindo a modernização gradual e integrada de ERPs legados, oferecendo uma solução escalável e eficiente.
