# Configuração da Integração com Google Sheets

Este documento explica como configurar a integração do sistema com Google Sheets para sincronizar automaticamente os dados cadastrados.

## Pré-requisitos

- Conta Google (Gmail)
- Acesso ao Google Cloud Console
- Permissões de administrador no sistema

## Passo 1: Criar Projeto no Google Cloud Console

1. Acesse [Google Cloud Console](https://console.cloud.google.com/)
2. Clique em **"Criar Projeto"**
3. Dê um nome ao projeto (ex: "Casa de Passagem")
4. Clique em **"Criar"**

## Passo 2: Habilitar API do Google Sheets

1. No menu lateral, vá em **"APIs e Serviços"** → **"Biblioteca"**
2. Busque por **"Google Sheets API"**
3. Clique na API e depois em **"Ativar"**

## Passo 3: Criar Conta de Serviço

1. No menu lateral, vá em **"APIs e Serviços"** → **"Credenciais"**
2. Clique em **"Criar Credenciais"** → **"Conta de serviço"**
3. Preencha os dados:
   - Nome: "Casa de Passagem Service"
   - ID: será gerado automaticamente
4. Clique em **"Criar e continuar"**
5. Em "Função", selecione **"Editor"**
6. Clique em **"Concluir"**

## Passo 4: Gerar Chave JSON

1. Na lista de contas de serviço, clique na conta que você acabou de criar
2. Vá na aba **"Chaves"**
3. Clique em **"Adicionar chave"** → **"Criar nova chave"**
4. Selecione **"JSON"**
5. Clique em **"Criar"**
6. Um arquivo JSON será baixado automaticamente - **GUARDE ESTE ARQUIVO COM SEGURANÇA**

## Passo 5: Criar Planilha no Google Sheets

1. Acesse [Google Sheets](https://sheets.google.com/)
2. Crie uma nova planilha
3. Dê um nome (ex: "Casa de Passagem - Dados")
4. Copie o ID da planilha da URL (é a parte entre `/d/` e `/edit`)
   - Exemplo: `https://docs.google.com/spreadsheets/d/1ABC123xyz/edit`
   - ID: `1ABC123xyz`

## Passo 6: Compartilhar Planilha com a Conta de Serviço

1. Na planilha criada, clique em **"Compartilhar"**
2. No arquivo JSON baixado, procure o campo `client_email`
3. Copie o email (será algo como: `nome@projeto.iam.gserviceaccount.com`)
4. Cole este email no campo de compartilhamento
5. Dê permissão de **"Editor"**
6. Clique em **"Enviar"**

## Passo 7: Configurar Variáveis de Ambiente no Sistema

1. Abra o arquivo JSON baixado
2. Localize os seguintes campos:
   - `client_email`
   - `private_key`
3. Acesse o painel de administração do sistema
4. Vá em **Configurações** → **Variáveis de Ambiente**
5. Adicione as seguintes variáveis:

```
GOOGLE_SHEETS_SPREADSHEET_ID=SEU_ID_DA_PLANILHA
GOOGLE_SHEETS_CLIENT_EMAIL=email_da_conta_de_servico
GOOGLE_SHEETS_PRIVATE_KEY=chave_privada_do_json
```

**Importante:** A chave privada deve incluir as quebras de linha (`\n`). Copie exatamente como está no arquivo JSON.

## Passo 8: Inicializar Estrutura da Planilha

1. No sistema, acesse o painel administrativo
2. Vá em **Configurações** → **Google Sheets**
3. Clique em **"Inicializar Estrutura"**
4. Aguarde a confirmação

Isso criará automaticamente as abas e cabeçalhos necessários:
- Aba "Acolhidos" com colunas: ID, Nome, CPF, Telefone, Foto URL, Data Cadastro
- Aba "Registros" com colunas: ID, Nome, CPF, Tipo, Data/Hora, Funcionário, Observações

## Passo 9: Testar Integração

1. Cadastre uma pessoa de teste no sistema
2. Verifique se os dados aparecem na planilha do Google Sheets
3. Registre uma entrada de teste
4. Verifique se o registro aparece na aba "Registros"

## Solução de Problemas

### Erro: "Permission denied"
- Verifique se a planilha foi compartilhada com o email da conta de serviço
- Confirme que a permissão é de "Editor", não apenas "Visualizador"

### Erro: "Invalid credentials"
- Verifique se copiou corretamente o `client_email` e `private_key`
- Certifique-se de que a chave privada inclui as quebras de linha (`\n`)

### Dados não aparecem na planilha
- Verifique se a API do Google Sheets está ativada no Google Cloud Console
- Confirme que o ID da planilha está correto
- Verifique os logs do sistema para mensagens de erro

### Erro: "Spreadsheet not found"
- Confirme que o ID da planilha está correto
- Verifique se a planilha não foi deletada ou movida

## Segurança

⚠️ **IMPORTANTE:**
- Nunca compartilhe o arquivo JSON com terceiros
- Não faça commit do arquivo JSON em repositórios públicos
- Mantenha as credenciais seguras e acessíveis apenas aos administradores
- Revogue e recrie as credenciais se houver suspeita de vazamento

## Backup dos Dados

Como os dados também ficam salvos no Google Sheets, você tem um backup automático. Recomendações:

1. Faça cópias mensais da planilha
2. Exporte em formato Excel (.xlsx) para arquivamento
3. Mantenha as cópias em local seguro

## Suporte

Para dúvidas sobre a configuração, entre em contato com o suporte técnico fornecendo:
- Descrição do problema
- Mensagens de erro (se houver)
- Prints da tela (sem expor credenciais)

---

**Versão:** 1.0  
**Última atualização:** Novembro 2025
