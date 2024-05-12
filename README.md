# Padrões de Projeto

## Criacional

### Builder

- Objetivo do padrão: tem como objetivo através de um processo gradual construir objetos complexos e permitir que um mesmo objetivo tenha variantes tendo o mesmo processo de construção.
- Qual problema resolve: evita que seja criado um número grande e desnecessário de subclasses ou que se crie um construtor que aborde todas as possibilidades, dessa maneira, na maioria dos casos se tem inúmeras chamadas inúteis.
- Solução: Para isso, são criados diversos objetos, chamados de “builder”, que são responsáveis por construir, assim, quando necessita-se construir um objeto, apenas são chamadas as etapas do objeto “builder” que são necessárias de acordo com o interesse. Além disso, também pode haver uma classe chamada de “diretor”, a fim de estabelecer a sequência em que as tarefas dos “builders” serão concretizadas.
- #### Diagrama UML:
<p align="center">
  <img src="https://github.com/enzo-s-azevedo/solid-principles/assets/142538641/c1eff34b-2147-41af-9cb9-dec2e3df1423" />
</p>

`Imagem extraída de:` https://refactoring.guru/pt-br/design-patterns/builder

- #### Explicação do código:
* `Product1` representa a estrutura do produto que será construído.
```c++
class Product1{
    public:
    std::vector<std::string> parts_;
    void ListParts()const{
        std::cout << "Product parts: ";
        for (size_t i=0;i<parts_.size();i++){
            if(parts_[i]== parts_.back()){
                std::cout << parts_[i];
            }else{
                std::cout << parts_[i] << ", ";
            }
        }
        std::cout << "\n\n"; 
    }
};
```
*	`Builder` é uma classe abstrata que define os métodos para criar o objeto.
```c++
class Builder{  
    public:
    virtual ~Builder(){}
    virtual void ProducePartA() const =0;
    virtual void ProducePartB() const =0;
    virtual void ProducePartC() const =0;
};
```
*	`ConcreteBuilder1` implementa a interface `Builder` de forma concreta.
```c++ 
class ConcreteBuilder1 : public Builder{
    private:

    Product1* product;

    public:

    ConcreteBuilder1(){
        this->Reset();
    }

    ~ConcreteBuilder1(){
        delete product;
    }

    void Reset(){
        this->product= new Product1();
    }


    void ProducePartA()const override{
        this->product->parts_.push_back("PartA1");
    }

    void ProducePartB()const override{
        this->product->parts_.push_back("PartB1");
    }

    void ProducePartC()const override{
        this->product->parts_.push_back("PartC1");
    }

    Product1* GetProduct() {
        Product1* result= this->product;
        this->Reset();
        return result;
    }
};
```
*	`Director` coordena a construção do objeto `Product` seguindo determinadas configurações.
```c++
class Director{

    private:
    Builder* builder;

    public:

    void set_builder(Builder* builder){
        this->builder=builder;
    }

    void BuildMinimalViableProduct(){
        this->builder->ProducePartA();
    }
    
    void BuildFullFeaturedProduct(){
        this->builder->ProducePartA();
        this->builder->ProducePartB();
        this->builder->ProducePartC();
    }
};
``` 
*	`ClientCode` informa como será construído o objeto produto através de comunicações com o `Builder` e `Director`.
```c++
void ClientCode(Director& director)
{
    ConcreteBuilder1* builder = new ConcreteBuilder1();
    director.set_builder(builder);
    std::cout << "Standard basic product:\n"; 
    director.BuildMinimalViableProduct();
    
    Product1* p= builder->GetProduct();
    p->ListParts();
    delete p;

    std::cout << "Standard full featured product:\n"; 
    director.BuildFullFeaturedProduct();

    p= builder->GetProduct();
    p->ListParts();
    delete p;

    std::cout << "Custom product:\n";
    builder->ProducePartA();
    builder->ProducePartC();
    p=builder->GetProduct();
    p->ListParts();
    delete p;

    delete builder;
}
```



## Estrutural

### Proxy

- Objetivo do padrão: proporciona um espaço reservado ou um substituto para outro objeto para moderar o acesso a esse objeto.
- Qual problema resolve: evita o alto consumo desnecessário de recursos do sistema e códigos duplicados.
- Solução: cria-se um objeto proxy que imita o objeto real e é responsável por interagir com o cliente. Assim, o objeto proxy atua como um intermediário realizando tarefas adicionais, como o controle ao acesso, e realizando as operações que seriam delegadas ao objeto real.
- #### Diagrama UML:
<p align="center">
  <img src="https://github.com/enzo-s-azevedo/solid-principles/assets/142538641/a7428143-e448-473f-8c80-259dff95732e" />
</p>

`Imagem extraída de:` https://refactoring.guru/pt-br/design-patterns/proxy

- #### Explicação do código:
*	`Subject` atribui operações comuns para o RealSubject e o Proxy e declara o método `Request()` que ambos devem implementar.
```c++
class Subject {
 public:
  virtual void Request() const = 0;
};
```
* `RealSubject` contém a lógica de negócio principal e implementa o método `Request()` que executa o trabalho real.
```c++
class RealSubject : public Subject {
 public:
  void Request() const override {
    std::cout << "RealSubject: Handling request.\n";
  }
};
```
*	`Proxy` é uma interface idêntica ao `RealSubject`, que atua como intermediária controlando o acesso através dos métodos:

   •`Request()`: implementa a operação `Request()` da interface `Subject`. Verifica o acesso usando `CheckAccess()`, se a verificação for bem-sucedida, encaminha a solicitação para o `RealSubject` e registra o acesso usando `LogAccess()`.
```c++
class Proxy : public Subject {

 private:
  RealSubject *real_subject_;

  bool CheckAccess() const {
    std::cout << "Proxy: Checking access prior to firing a real request.\n";
    return true;
  }
  void LogAccess() const {
    std::cout << "Proxy: Logging the time of request.\n";
  }

 public:
  Proxy(RealSubject *real_subject) : real_subject_(new RealSubject(*real_subject)) {
  }

  ~Proxy() {
    delete real_subject_;
  }

  void Request() const override {
    if (this->CheckAccess()) {
      this->real_subject_->Request();
      this->LogAccess();
    }
  }
};
```
*	ClientCode: demonstra a tentativa do cliente interagir com o `Subject`, invocando a operação `Request()` no `Subject`.
```c++
void ClientCode(const Subject &subject) {
 
  subject.Request();

}  
```


## Comportamental

### Mediator

- Objetivo do padrão: tem como objetivo usar um objeto mediador para evitar comunicações diretas entre os objetos, assim as dependências entre eles são reduzidas.
- Qual problema resolve: impede que haja um código muito complexo, evita um alto acoplamento desnecessário entre os objetos e promove a extensibilidade do código.
- Solução: os objetos ficam dependentes apenas do objeto mediador, o qual é responsável por redirecionar as chamadas para os objetos apropriados. Assim, a dependência entre os objetos é reduzida e o código torna-se mais extensível e menos complexo. 
- #### Diagrama UML:
<p align="center">
  <img src="https://github.com/enzo-s-azevedo/solid-principles/assets/142538641/ca8f725a-a031-411f-9bdf-12590960e390" />
</p>

`Imagem extraída de:` https://refactoring.guru/pt-br/design-patterns/mediator

- #### Explicação do código:


### Todo o conteúdo desse repositório usou como refererência https://refactoring.guru/pt-br/design-patterns/

