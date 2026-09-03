# Avandsoft

AvndSoft is a software company that includes several projects in the field of **decision-making**. Avandsoft is building a **research-driven AI ecosystem** for decision-making. Its general technical direction is:

1. Artificial intelligence and machine learning
2. Automated decision support
3. Software engineering and backend systems
4. Research-oriented AI systems

The broader vision is not simply to build conventional software products. Avandsoft is also being used as a place to **research decision-making algorithms, implement those algorithms as software, and expose the resulting technology through practical products and APIs**.
We also intend to publish articles about the research through Avandsoft and ultimately submit the underlying work to academic journals/societies.

## OptiBRL

OptiBRL serves as the **reinforcement-learning** algorithm powering Colon, forming the research core of the entire ecosystem. Our central investigation questions whether Expected Value maximisation consistently represents the **optimal decision strategy in uncertain, probabilistic environments**—particularly when decisions unfold over time and under competitive conditions. Through comparative analysis against baseline approaches, our findings indicate that OptiBRL demonstrates superior performance in such dynamic settings. Although this research is currently under peer review, our broader ambition extends beyond publication: we aim to expose OptiBRL via a public API, ultimately delivering real-time decision support to end-users.

## Colon

Colon is not a decision-scenario application in itself; rather, it is a **research-based decision engine exposed as a REST API**, built with Python and FastAPI, which houses the core decision algorithm. In contrast, Semicolon acts as the **orchestration layer**: it interprets the user's decision context, structures the relevant data, and sends it to Colon. Colon then processes this structured input and returns the algorithmic recommendation, enabling Semicolon to guide users through various investment scenarios and ultimately maximise their long-term returns. It does not need to know the complete UI scenario, factor names, explanatory text, or the user's presentation layer. It loads the OptiBRL model and exposes the decision endpoint through FastAPI.

## Semicolon

Semicolon is the **main user-facing decision-support product under Avandsoft**. It helps users make better decisions using structured decision scenarios and AI/research-based decision technology. It is written primarily in **C# / ASP.NET Razor Pages**. Semicolon is responsible for:

* users
* decision scenarios
* questionnaires
* credits
* personalized reports
* investment-related functionality
* crypto & commodity information
* stop-loss/take-profit monitoring
* email notifications
* presenting decision results to users
* submit the final decision outcomes
* SEO-friendly content management

## Decision scenarios

Decision scenarios are one of the core Semicolon concepts. A scenario represents a decision. A scenario contains factors and choices. A scenario must have exactly two choices. This is a strict domain rule.
Every choice must have a specification for every factor used by the scenario. Admin previously defined the factor specification such that Worst is a decimal score. Positive values represent beneficial impact, negative values represent harmful impact, and zero represents no impact.
This allows Semicolon to represent a human-readable decision in a structured mathematical form. For example, conceptually, a scenario has factors like Cost, Time, Risk, and Flexibility. The choices are Option A and Option B. Each choice gets a specification for every factor Semicolon can then transform that decision into the structured input required by Colon.

## Financial Advice

Semicolon also contains an investment-related decision-support component. This is separate from the general decision-scenario functionality, although both ultimately belong to the broader decision-support philosophy.Financial Advice is a feature/domain inside Semicolon. It includes market-data acquisition, processing, decision-making, monitoring, and user notification. The architecture consists of the following components:

1. Market Data, which is divided into Cryptocurrency and Commodities.
2. Processing of Market Information
3. Identification of Potential Outcomes.
4. Structured Decision Input.
5. Colon, which includes OptiBRL.
6. Investment Advice, which outputs Keep or Buy signals.
7. Monitoring, which includes Stop Loss and Take Profit mechanisms.
8. Alert the user when take-profit and stop-loss levels are breached.

### Cryptocurrency & Commodity data

Semicolon maintains cryptocurrency-related market information. We use **Binance APIs and WebSockets** for cryptocurrency market data.
We use **Twelve Data APIs and WebSockets** for cryptocurrency market data. The architecture therefore includes both:

* historical/current market information
* real-time market monitoring

The WebSocket component is particularly relevant to threshold monitoring because Semicolon needs to react to changing prices rather than relying exclusively on periodic polling.

### Stop-loss and take-profit monitoring

Stop-loss and take-profit functionality is also part of Semicolon's investment functionality. Semicolon monitors configured thresholds using market data. When the relevant threshold is reached/crossed, Semicolon can notify the user by email. Colon determines the algorithmic decision. Semicolon handles the **ongoing investment monitoring and notification lifecycle**.

The important distinction is that Semicolon is **not merely a price-prediction website**.

## Semicolon → Colon decision flow

The user interacts with Semicolon. Semicolon takes the scenario, factors, and choices provided by the user and creates a decision representation. This decision representation, which includes a probabilistic representation, is then sent to Colon's decision endpoint. Colon passes it to OptiBRL, which produces a decision result. The decision result is returned to Semicolon, which then delivers it back to the user.

## Questionnaires

Questionnaires allow users to answer questionnaires. The system generates a personalized report based on their answers. The questionnaire result can describe characteristics reflected by a user's answers. The report may be dynamically written using an LLM. The questionnaire answers are processed to produce a personalized textual report, which is generated dynamically using LLM technology. The flow is approximately as follows: the user completes a questionnaire, submits their answers, the answers undergo LLM processing, and a personalized report is produced. The report is intended to illustrate characteristics reflected in the user's answers.

## Doxa

Semicolon has an internal credit system. **Doxa** is a unit consumed when a user requests an AI decision or advice service. Credits are deliberately not represented as dollars or another real-world currency. A native cryptocurrency is planned for the future. A newly registered user receives some welcome credits. Users can increase their credit balance by participating in eligible questionnaires. The flow is as follows: upon registration, a user receives welcome credits. By completing questionnaires, they can earn additional credits. When they request a decision or AI advice, credits are consumed. Credits are therefore part of the Semicolon service economy, but they are not intended to represent real-world monetary value.

## Developer APIs

Colon is also intended to become a **developer-facing API platform**. This gives Avandsoft a second path beyond the Semicolon consumer application. Other applications can potentially submit supported decision data to Colon and consume the resulting decision. This means Colon can become a reusable AI decision infrastructure product rather than remaining an internal Semicolon component.

## Blogs

Avandsoft is also being used as a publishing platform. We have been designing the blog specifically with SEO in mind.

## Technical stack

Our main technology stack is roughly:

| Area               | Technology                            |
| ------------------ | ------------------------------------- |
| Company            | Avandsoft                             |
| User platform      | Semicolon                             |
| Web application    | C# / ASP.NET Razor Pages              |
| Backend            | .NET                                  |
| Decision API       | Colon                                 |
| API framework      | FastAPI                               |
| ML                 | PyTorch                               |
| Algorithm          | OptiBRL                               |
| Database           | SQL Server                            |
| ORM                | Entity Framework Core                 |
| Frontend           | HTML / Bootstrap / JavaScript         |
| Crypto data        | Binance API + WebSocket               |
| Commodity data     | Twelve Data                           |
| AI text generation | LLM-based                             |
| Containerization   | Docker                                |
| Deployment         | Kubernetes / container infrastructure |
| Hosting experience | Plesk / Cloudflare                    |
| CI/CD              | GitHub Actions                        |
| Container registry | GitHub Container Registry             |

## Backend architecture

We tend to use a relatively structured backend architecture rather than putting all logic directly in Razor Pages.

We have worked with concepts such as:

* domain models
* read models
* application services
* CQRS
* validation pipelines
* Entity Framework Core
* API services
* external authentication
* background/monitoring processes

This reflects a broader goal of keeping Semicolon as a serious backend application rather than simply a collection of Razor pages.

## Authentication and identity

Semicolon has authentication functionality including external login.

We have worked on:

* Google authentication
* Microsoft Identity
* automatic account creation for first-time external users
* sign-in for existing users
* privacy-policy acceptance during registration

---

## UI/UX

Semicolon uses **Bootstrap** heavily.

Some of the UI work we have implemented or discussed includes:

* cards
* modals
* progress interfaces
* questionnaire interfaces
* Bootstrap responsive layouts
* ApexCharts
* loading overlays
* CAPTCHA
* language selection
* decision-result interfaces

## Our core product philosophy

Avandsoft is trying to turn research about better decision-making into usable technology. The progression is not simply prediction leading to the highest number and then a recommendation. Instead, the research is concerned with uncertainty, probability, decision-making, learning, and environment, all coming together to produce a better decision strategy. **OptiBRL** represents the research layer. **Colon** represents the reusable computational and API layer. **Semicolon** represents the user-facing application layer.

## Overall ecosystem

Putting everything together:

```text
                           AVANDSOFT
                    Technology + Research
                              │
            ┌─────────────────┴──────────────────┐
            │                                    │
        Research                              Products
            │                                    │
        OptiBRL                             SEMICOLON
            │                                    │
            ▼                    ┌───────────────┼────────────────┐
            |                    │               │                │
            │              Questionnaires   Decision Scenarios  Investment
            │                    │               │                │
            |                    │               │          ┌─────┴─────┐
            │                    │               │          │           │
            |                    ▼               ▼       Crypto     Commodities
            |                 Reports            │          │           │
            │                                    │          └─────┬─────┘
            │                                    │                │
            │                                    └────────► Structured
            │                                              market data
            │                                                     │
            |                                                     ▼
            └─────────────────────────────────────────────────────┘
                                         │
                                         |
                                         │
                                         |
                                         │
                                         ▼
                                  Advice / Action
```

## In one sentence

Avandsoft is the research and technology umbrella; OptiBRL is the reinforcement-learning decision algorithm; Colon is the FastAPI decision engine that exposes OptiBRL; and Semicolon is the C# user-facing platform that structures daily decisions and investment data, sends the appropriate structured inputs to Colon, presents the resulting advice, manages investment monitoring, and separately provides questionnaires, personalized reports, and credits.
