# Mercado.py
# É uma aplicação web interativa, criada com Python, que simula uma compra de mercado.
# O usuário escolhe produtos, informa quantidades, e o sistema calcula o valor total da compra, exibindo mensagens dinâmicas conforme o valor.
# Lista de produtos e preços

from shiny import App, ui, render, reactive

produtos = {
    "Arroz": 5.00,
    "Feijão": 7.00,
    "Macarrão": 4.50,
    "Açúcar": 3.20,
    "Café": 8.90
}

app_ui = ui.page_fluid(
    ui.h2("Simulador de Compras - Mercado"),

    ui.input_checkbox_group("itens", "Escolha os produtos:", list(produtos.keys())),

    ui.output_ui("quantidades_ui"),

    ui.output_text("total_compra"),

    ui.output_text("mensagem_final")
)

def server(input, output, session):

    @output
    @render.ui
    def quantidades_ui():
        # Cria inputs de quantidade apenas para os produtos selecionados
        return [
            ui.input_numeric(f"qtd_{item}", f"Quantidade de {item}:", 1, min=1)
            for item in input.itens()
        ]

    @output
    @render.text
    def total_compra():
        total = 0
        for item in input.itens():
            qtd = input[f"qtd_{item}"]() or 0  # ✅ CORRIGIDO
            preco = produtos[item]
            total += qtd * preco
        return f"Total da compra: R$ {total:.2f}"

    @output
    @render.text
    def mensagem_final():
        total = 0
        for item in input.itens():
            qtd = input[f"qtd_{item}"]() or 0  # ✅ CORRIGIDO
            preco = produtos[item]
            total += qtd * preco

        if total >= 100:
            return "🎉 Parabéns! Você ganhou um desconto de 10%!"
        elif total == 0:
            return "🛒 Selecione ao menos um produto para iniciar a compra."
        else:
            return "Obrigado por comprar conosco!"

app = App(app_ui, server)
