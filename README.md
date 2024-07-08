# Desafio Dio Criando um App Gerenciador de Estoque com Ruby



**Projeto completo e abrangente para Criando um App Gerenciador de Estoque com Ruby**

**Projeto de Aplicativo Gerenciador de Estoque com Ruby**

**Objetivo:** Criar um aplicativo gerenciador de estoque abrangente usando a linguagem de programação Ruby.



### **Requisitos:**

- Conhecimento básico de Ruby
- Banco de dados (por exemplo, PostgreSQL, MySQL)
- Editor de código (por exemplo, Visual Studio Code, Sublime Text)

**Etapas:**



### **1. Configuração do Banco de Dados**



Crie um banco de dados e uma tabela para armazenar os dados do estoque:

sql



```sql
CREATE TABLE products (
  id SERIAL PRIMARY KEY,
  name VARCHAR(255) NOT NULL,
  description TEXT,
  category VARCHAR(255),
  quantity INTEGER NOT NULL DEFAULT 0,
  price DECIMAL(10, 2) NOT NULL DEFAULT 0.00,
  created_at TIMESTAMP NOT NULL DEFAULT NOW(),
  updated_at TIMESTAMP NOT NULL DEFAULT NOW()
);

CREATE TABLE categories (
  id SERIAL PRIMARY KEY,
  name VARCHAR(255) NOT NULL,
  description TEXT
);

CREATE TABLE stock_histories (
  id SERIAL PRIMARY KEY,
  product_id INTEGER NOT NULL,
  quantity INTEGER NOT NULL,
  type VARCHAR(255) NOT NULL,
  created_at TIMESTAMP NOT NULL DEFAULT NOW()
);

ALTER TABLE products
ADD FOREIGN KEY (category) REFERENCES categories (name);

ALTER TABLE stock_histories
ADD FOREIGN KEY (product_id) REFERENCES products (id);
```



### **2. Criação dos Modelos**



Crie os modelos para representar os dados do produto, categoria e histórico de estoque:

ruby



```ruby
class Product < ApplicationRecord
  belongs_to :category
  has_many :stock_histories

  validates :name, presence: true
  validates :quantity, presence: true, numericality: { greater_than_or_equal_to: 0 }
  validates :price, presence: true, numericality: { greater_than_or_equal_to: 0 }
end

class Category < ApplicationRecord
  has_many :products
end

class StockHistory < ApplicationRecord
  belongs_to :product

  validates :product_id, presence: true
  validates :quantity, presence: true, numericality: { greater_than_or_equal_to: 0 }
  validates :type, presence: true
end
```



### **3. Criação dos Controladores**

Crie os controladores para gerenciar as ações relacionadas a produtos, categorias e histórico de estoque:



### **ProductsController:**

ruby



```ruby
class ProductsController < ApplicationController
  def index
    @products = Product.all
  end

  def new
    @product = Product.new
  end

  def create
    @product = Product.new(product_params)

    if @product.save
      redirect_to products_path, notice: 'Produto criado com sucesso.'
    else
      render :new
    end
  end

  def edit
    @product = Product.find(params[:id])
  end

  def update
    @product = Product.find(params[:id])

    if @product.update(product_params)
      redirect_to products_path, notice: 'Produto atualizado com sucesso.'
    else
      render :edit
    end
  end

  def destroy
    @product = Product.find(params[:id])
    @product.destroy

    redirect_to products_path, notice: 'Produto excluído com sucesso.'
  end

  private

    def product_params
      params.require(:product).permit(:name, :description, :category, :quantity, :price)
    end
end
```



### **CategoriesController:**

ruby



```ruby
class CategoriesController < ApplicationController
  def index
    @categories = Category.all
  end

  def new
    @category = Category.new
  end

  def create
    @category = Category.new(category_params)

    if @category.save
      redirect_to categories_path, notice: 'Categoria criada com sucesso.'
    else
      render :new
    end
  end

  def edit
    @category = Category.find(params[:id])
  end

  def update
    @category = Category.find(params[:id])

    if @category.update(category_params)
      redirect_to categories_path, notice: 'Categoria atualizada com sucesso.'
    else
      render :edit
    end
  end

  def destroy
    @category = Category.find(params[:id])
    @category.destroy

    redirect_to categories_path, notice: 'Categoria excluída com sucesso.'
  end

  private

    def category_params
      params.require(:category).permit(:name, :description)
    end
end
```



### **StockHistoriesController:**

ruby



```ruby
class StockHistoriesController < ApplicationController
  def index
    @stock_histories = StockHistory.all
  end

  def new
    @stock_history = StockHistory.new
  end

  def create
    @stock_history = StockHistory.new(stock_history_params)

    if @stock_history.save
      redirect_to stock_histories_path, notice: 'Histórico de estoque criado com sucesso.'
    else
      render :new
    end
  end

  def edit
    @stock_history = StockHistory.find(params[:id])
  end

  def update
    @stock_history = StockHistory.find(params[:id])

    if @stock_history.update(stock_history_params)
      redirect_to stock_histories_path, notice: 'Histórico de estoque atualizado com sucesso.'
    else
      render :edit
    end
  end

  def destroy
    @stock_history = StockHistory.find(params[:id])
    @stock_history.destroy

    redirect_to stock_histories_path, notice: 'Histórico de estoque excluído com sucesso.'
  end

  private

    def stock_history_params
      params.require(:stock_history).permit(:product_id, :quantity, :type)
    end
end
```



### **4. Criação das Views**

Crie as views para exibir e editar os dados do produto, categoria e histórico de estoque:



### **index.html.erb (Produtos):**

erb



```erb
<h1>Produtos</h1>

<table class="table">
  <thead>
    <tr>
      <th>Nome</th>
      <th>Descrição</th>
      <th>Categoria</th>
      <th>Quantidade</th>
      <th>Preço</th>
      <th>Ações</th>
    </tr>
  </thead>

  <tbody>
    <% @products.each do |product| %>
      <tr>
        <td><%= product.name %></td>
        <td><%= product.description %></td>
        <td><%= product.category.name %></td>
        <td><%= product.quantity %></td>
        <td><%= product.price %></td>
        <td>
          <%= link_to 'Editar', edit_product_path(product), class: 'btn btn-primary' %>
          <%= link_to 'Excluir', product_path(product), method: :delete, data: { confirm: 'Tem certeza que deseja excluir este produto?' }, class: 'btn btn-danger' %>
        </td>
      </tr>
    <% end %>
  </tbody>
</table>

<%= link_to 'Novo Produto', new_product_path, class: 'btn btn-success' %>
```



### **new.html.erb (Produtos):**

erb

![Done](chrome-extension://igpdmclhhlcpoindmhkhillbfhdgoegm/b3baca6de20012788f7d.svg)![Copy](chrome-extension://igpdmclhhlcpoindmhkhillbfhdgoegm/7120b68615ebe4b28075.svg)

```erb
<h1>Novo Produto</h1>

<%= render 'form' %>

<%= link_to 'Voltar', products_path, class: 'btn btn-secondary' %>
```



### **edit.html.erb (Produtos):**

erb



```erb
<h1>Editar Produto</h1>

<%= render 'form' %>

<%= link_to 'Voltar', products_path, class: 'btn btn-secondary' %>
```



### *_form.html.erb (Produtos):**

erb



```erb
<%= form_with(model: @product) do |form| %>
  <div class="form-group">
    <%= form.label :name %>
    <%= form.text_field :name, class: 'form-control' %>
  </div>

  <div class="form-group">
    <%= form.label :description %>
    <%= form.text_area :description, class: 'form-control' %>
  </div>

  <div class="form-group">
    <%= form.label :category %>
    <%= form.select :category_id, Category.all.collect { |c| [c.name, c.id] }, { include_blank: true }, class: 'form-control' %>
  </div>

  <div class="form-group">
    <%= form.label :quantity %>
    <%= form.number_field :quantity, class: 'form-control' %>
  </div>

  <div class="form-group">
    <%= form.label :price %>
    <%= form.number_field :price, class: 'form-control' %>
  </div>

  <div class="form-group">
    <%= form.submit class: 'btn btn-primary' %>
  </div>
<% end %>
```



### **index.html.erb (Categorias):**

erb



```erb
<h1>Categorias</h1>

<table class="table">
  <thead>
    <tr>
      <th>Nome</th>
      <th>Descrição</th>
      <th>Ações</th>
    </tr>
  </thead>

  <tbody>
    <% @categories.each do |category| %>
      <tr>
        <td><%= category.name %></td>
        <td><%= category.description %></td>
        <td>
          <%= link_to 'Editar', edit_category_path(category), class: 'btn btn-primary' %>
          <%= link_to 'Excluir', category_path(category), method: :delete, data: { confirm: 'Tem certeza que deseja excluir esta categoria?' }, class: 'btn btn-danger' %>
        </td>
      </tr>
    <% end %>
  </tbody>
</table>

<%= link_to 'Nova Categoria', new_category_path, class: 'btn btn-success' %>
```



### **new.html.erb (Categorias):**

erb



```erb
<h1>Nova Categoria</h1>

<%= render 'form' %>

<%= link_to 'Voltar', categories_path, class: 'btn btn-secondary' %>
```



### **edit.html.erb (Categorias):**

erb



```erb
<h1>Editar Categoria</h1>

<%= render 'form' %>

<%= link_to 'Voltar', categories_path, class: 'btn btn-secondary' %>
```



### **_form.html.erb (Categorias):**

erb



```erb
<%= form_with(model: @category) do |form| %>
  <div class="form-group">
    <%= form.label :name %>
    <%= form.text_field :name, class: 'form-control' %>
  </div>

  <div class="form-group">
    <%= form.label :description %>
    <%= form.text_area :description, class: 'form-control' %>
  </div>

  <div class="form-group">
    <%= form.submit class: 'btn btn-primary' %>
  </div>
<% end %>
```



### **index.html.erb (Histórico de Estoque):**

erb



```erb
<h1>Histórico de Estoque</h1>

<table class="table">
  <thead>
    <tr>
      <th>Produto</th>
      <th>Quantidade</th>
      <th>Tipo</th>
      <th>Data</th>
      <th>Ações</th>
    </tr>
  </thead>

  <tbody>
    <% @stock_histories.each do |stock_history| %>
      <tr>
        <td><%= stock_history.product.name %></td>
        <td><%= stock_history.quantity %></td>
        <td><%= stock_history.type %></td>
        <td><%= stock_history.created_at.strftime('%d/%m/%Y %H:%M') %></td>
        <td>
          <%= link_to 'Editar', edit_stock_history_path(stock_history), class: 'btn btn-primary' %>
          <%= link_to 'Excluir', stock_history_path(stock_history), method: :delete, data: { confirm: 'Tem certeza que deseja excluir este histórico de estoque?' }, class: 'btn btn-danger' %>
        </td>
      </tr>
    <% end %>
  </tbody>
</table>

<%= link_to 'Novo Histórico de Estoque', new_stock_history_path, class: 'btn btn-success' %>
```



### **new.html.erb (Histórico de Estoque):**

erb



```erb
<h1>Novo Histórico de Estoque</h1>

<%= render 'form' %>

<%= link_to 'Voltar', stock_histories_path, class: 'btn btn-secondary' %>
```



### **edit.html.erb (Histórico de Estoque):**

erb



```erb
<h1>Editar Histórico de Estoque</h1>

<%= render 'form' %>

<%= link_to 'Voltar', stock_histories_path, class: 'btn btn-secondary' %>
```



### **_form.html.erb (Histórico de Estoque):**

erb



```erb
<%= form_with(model: @stock_history) do |form| %>
  <div class="form-group">
    <%= form.label :product_id %>
    <%= form.select :product_id, Product.all.collect { |p| [p.name, p.id] }, { include_blank: true }, class: 'form-control' %>
  </div>

  <div class="form-group">
    <%= form.label :quantity %>
    <%= form.number_field :quantity, class: 'form-control' %>
  </div>

  <div class="form-group">
    <%= form.label :type %>
    <%= form.select :type, ['Entrada', 'Saída'], class: 'form-control' %>
  </div>

  <div class="form-group">
    <%= form.submit class: 'btn btn-primary' %>
  </div>
<% end %>
```



### **5. Rotas**

Adicione as rotas para o gerenciamento de produtos, categorias e histórico de estoque:

ruby



```ruby
Rails.application.routes.draw do
  resources :products
  resources :categories
  resources :stock_histories
  root to: 'products#index'
end
```



### **6. Inicialização do Banco de Dados**

Execute as migrações para criar as tabelas de produtos, categorias e histórico de estoque:

plaintext



```plaintext
bundle exec rails db:migrate
```



### **7. Execução do Aplicativo**



Execute o aplicativo:

plaintext



```plaintext
bundle exec rails s
```



### **Conclusão:**



Este projeto fornece um aplicativo gerenciador de estoque abrangente que permite adicionar, editar e excluir produtos, categorias e histórico de estoque. Você pode estender este projeto adicionando recursos adicionais, como relatórios avançados, gerenciamento de usuários e integração com sistemas externos.
