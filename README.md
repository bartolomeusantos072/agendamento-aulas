# Agendamento de Aulas Particulares

Site para divulgação e agendamento de aulas particulares.

## Sobre o projeto

O site apresenta informações do professor, serviços oferecidos e permite que o aluno solicite um agendamento.

Os dados do professor e dos serviços são carregados pelo arquivo `data.json`.

## Tecnologias

* HTML
* CSS
* JavaScript
* JSON
* SheetDB
* Google Sheets
* WhatsApp
* GitHub Pages

## Estrutura

```text
├── index.html
├── style.css
├── script.js
├── data.json
├── README.md
└── imagens/
    └── foto_perfil.jpeg
```

## Funcionamento

O aluno:

1. Acessa o site.
2. Escolhe uma disciplina ou serviço.
3. Informa a data e o horário desejados.
4. Envia o formulário.
5. O agendamento é registrado no Google Sheets através do SheetDB.
6. O aluno é direcionado para o WhatsApp.

## Personalização

As informações do profissional podem ser alteradas no arquivo `data.json`, como:

* Nome
* Descrição
* Foto
* WhatsApp
* Serviços
* Preços

O mesmo projeto pode ser adaptado para outros tipos de negócio, como:

* Salão de beleza
* Barbearia
* Manicure
* Oficina
* Personal trainer
* Outros serviços

## Publicação

O projeto pode ser publicado gratuitamente utilizando o GitHub Pages.
