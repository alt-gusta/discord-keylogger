# Keylogger Didático com Relatório via Discord

![Propósito](https://img.shields.io/badge/Propósito-Estritamente%20Educacional-blue.svg)
![Linguagem](https://img.shields.io/badge/Linguagem-Python-3776AB?logo=python)
![Licença](https://img.shields.io/badge/Licença-MIT-green.svg)

Este repositório contém o código-fonte de um keylogger funcional, desenvolvido em Python, com o **único e exclusivo propósito de servir como uma ferramenta de estudo em cibersegurança**.

O script foi projetado para servir como material de apoio para um palestra de cibersegurança, demostrando de forma prática e clara os mecanismos utilizados em malwares reais para capturar informações e exfiltrar dados, permitindo que estudantes e entusiastas de segurança compreendam as ameaças para melhor combatê-las.

---

## ⚠️ AVISO IMPORTANTE: USO ESTRITAMENTE EDUCACIONAL ⚠️

> **NUNCA UTILIZE ESTE CÓDIGO EM UM COMPUTADOR QUE NÃO SEJA SEU OU SEM A AUTORIZAÇÃO EXPLÍCITA E POR ESCRITO DO PROPRIETÁRIO.**
>
> O uso deste software para interceptar dados de terceiros sem consentimento é **crime**, uma grave violação de privacidade e pode ter sérias consequências legais.
>
> Este projeto foi desenvolvido para que estudantes e profissionais de cibersegurança possam **entender o mecanismo de funcionamento** de um keylogger, a fim de criar e aprimorar métodos de defesa, detecção e prevenção.
>
> **O autor e os contribuidores deste projeto não se responsabilizam, sob nenhuma circunstância, pelo mau uso ou por quaisquer danos causados pela utilização indevida deste código.**

---

## Funcionalidades e Pontos de Estudo

Este código não é apenas um script básico, mas implementa várias técnicas que servem como excelentes pontos de estudo:

* **Captura de Teclas Assíncrona:** Utiliza a biblioteca `keyboard` para monitorar eventos de teclado em segundo plano.
* **Relatórios Periódicos:** Emprega `threading.Timer` para agrupar as teclas capturadas e enviá-las em intervalos definidos, uma técnica para evitar tráfego de rede constante e suspeito.
* **Exfiltração via Discord Webhook:** Usa uma plataforma comum (Discord) como um canal de Comando e Controle (C2) para receber os dados, demonstrando como serviços legítimos podem ser abusados.
* **Gerenciamento de Logs:**
    * Para logs curtos, envia os dados em um `Embed` do Discord, para fácil visualização.
    * Para logs longos (acima de 2000 caracteres), cria um arquivo de texto temporário, faz o upload e o exclui em seguida, bypassando limites de caracteres e removendo vestígios.
* **Coleta de Metadados:** Captura o nome de usuário (`os.getlogin()`) e timestamps para contextualizar os dados.
* **Formatação de Teclas Especiais:** Converte teclas como `space`, `enter` e `shift` em um formato legível no log final.
* **Gerenciamento de Credenciais:** Utiliza um arquivo `.env` para carregar a URL do webhook, uma boa prática para não expor informações sensíveis diretamente no código.

## Como o Código Funciona

1.  **Inicialização (`Keylogger.__init__`)**: A classe é instanciada, definindo o intervalo dos relatórios. O nome de usuário e o horário de início são registrados.
2.  **Captura (`callback`)**: A cada tecla liberada (`on_release`), a função `callback` é chamada. Ela formata o nome da tecla e a adiciona à variável `self.log`.
3.  **Agendamento (`report`)**: Um `Timer` é iniciado. Ao final do intervalo, ele chama a função `report_to_webhook`. O log é limpo e o `Timer` é reiniciado, criando um ciclo contínuo.
4.  **Relatório (`report_to_webhook`)**: Esta é a função de exfiltração. Ela verifica o tamanho do log. Se for maior que 2000 caracteres, salva o conteúdo em `log.txt` na pasta temporária do sistema, envia o arquivo via `DiscordWebhook` e depois o apaga. Caso contrário, envia um `DiscordEmbed` com o log.

## Como Estudar o Código (Ambiente Controlado)

**É crucial executar este código apenas em um ambiente que você controla e possui, preferencialmente uma máquina virtual (VM) isolada.**

1.  **Clone o repositório:**
    ```bash
    git clone [https://github.com/seu-usuario/seu-repositorio.git](https://github.com/seu-usuario/seu-repositorio.git)
    cd seu-repositorio
    ```

2.  **Crie e ative um ambiente virtual:**
    ```bash
    python -m venv venv
    # Windows
    .\venv\Scripts\activate
    # Linux / macOS
    source venv/bin/activate
    ```

3.  **Instale as dependências:**
    ```bash
    pip install -r requirements.txt
    ```

4.  **Configure o Webhook:**
    * Crie um arquivo chamado `.env` na raiz do projeto.
    * Dentro dele, adicione a URL do seu webhook do Discord:
        ```
        DISCORD_WEBHOOK_URL="[https://discord.com/api/webhooks/seu_id/seu_token](https://discord.com/api/webhooks/seu_id/seu_token)"
        ```
    * **Importante:** Adicione o arquivo `.env` ao seu `.gitignore` para nunca cometer suas credenciais ao repositório.

5.  **Execute e Analise:**
    ```bash
    python keylogger.py
    ```
    Digite alguns textos na sua máquina virtual e observe como os relatórios chegam ao seu canal do Discord no intervalo configurado. Analise o código para entender cada passo do processo.

## Exoneração de Responsabilidade (Disclaimer)

Este software é fornecido "como está", sem garantia de qualquer tipo, expressa ou implícita. O objetivo é puramente educacional. A utilização deste código para atividades que violem leis locais ou internacionais é de inteira responsabilidade do usuário.

## Licença

Este projeto está licenciado sob a Licença MIT. Veja o arquivo `LICENSE` para mais detalhes.