# Princípios SOLID em Java

Os principios de SOLID são um conjunto de 5 principios que ajudam a escrever código mais limpo e mais facil de manter.

S - Single Responsibility Principle

O - Open/Closed Principle

L - Liskov Substitution Principle

I - Interface Segregation Principle

D - Dependency Inversion Principle

### Single Responsibility Principle
Princípio da Responsabilidade Única (SRP)
O SRP estabelece que uma classe deve ter uma única razão para mudar.
Em outras palavras, uma classe deve ter apenas uma responsabilidade.
Isso facilita a manutenção e evita que a classe se torne complexa e difícil de entender.

Exemplo incorreto:
```Java
private class PaymentHandler
	{
		public void processPayment(Payment payment)
		{
			// Lógica para processar o pagamento
			// Lógica para registrar informações de pagamento
		}
	}
```
A classe PaymentHandler tem duas responsabilidades: processar pagamentos e registrar informações de pagamento.
Isso viola o princípio SRP, pois a classe tem mais de uma razão para mudar.

Exemplo correto:
```Java
class PaymentProcessor
	{
		public void processPayment(Payment payment)
		{
			// Lógica para processar o pagamento
		}
	}

	class PaymentLogger
	{
		public void logPayment(Payment payment)
		{
			// Lógica para registrar informações de pagamento
		}
	}
  ```
Para corrigir o exemplo anterior, podemos separar as responsabilidades em classes diferentes.
Uma classe PaymentProcessor é responsável por processar pagamentos, enquanto uma classe PaymentLogger
é responsável por registrar informações de pagamento. Dessa forma, cada classe tem uma única responsabilidade.

### Open/Closed Principle
Princípio Aberto/Fechado (OCP)
O OCP prega que as entidades de software (classes, módulos, etc.)
devem estar abertas para extensão, mas fechadas para modificação.

Isso significa que você deve conseguir adicionar novos recursos sem alterar o código existente.

Exemplo incorreto:
```Java
class Shape
    {
        // Implementação genérica para todas as formas
    }

    class Circle extends Shape
    {
        // Lógica específica para cálculo de área de círculo
    }

    class Rectangle extends Shape
    {
        // Lógica específica para cálculo de área de retângulo
    }
```
Caso seja necessário adicionar uma nova forma, será necessário modificar a classe Shape
para adicionar um novo método de cálculo de área. Isso viola o princípio OCP.

Exemplo correto:
```Java
interface Shape
    {
        double calculateArea();
    }

    class Circle implements Shape
    {
        private double radius;

        // Construtor, getters e setters

        @Override
        public double calculateArea()
        {
            return Math.PI * radius * radius;
        }
    }

    class Rectangle implements Shape
    {
        private double width;
        private double height;

        // Construtor, getters e setters

        @Override
        public double calculateArea()
        {
            return width * height;
        }
    }
```
Para corrigir o exemplo anterior, podemos usar uma interface Shape com um método calculateArea().
Assim, cada forma implementa a interface e fornece sua própria lógica de cálculo de área.
Dessa forma, podemos adicionar novas formas sem modificar o código existente.

### Liskov Substitution Principle
Princípio da Substituição de Liskov (LSP)
O LSP estabelece que objetos de uma superclasse devem ser substituíveis por objetos de suas subclasses
sem afetar a integridade do programa. Em outras palavras, uma classe derivada deve poder substituir
sua classe base sem que o programa se comporte de maneira inesperada.

Exemplo incorreto:
```Java
class Bird
    {
        void fly()
        {
            // Lógica para voar
        }
    }

    class Ostrich extends Bird
    {
        void fly()
        {
            throw new UnsupportedOperationException("As avestruzes não voam");
        }
    }
```
Aqui temos uma classe Bird com um método fly() que é substituído pela classe Ostrich para lançar uma exceção.
Isso viola o princípio LSP, pois a classe derivada não pode substituir a classe base sem afetar o comportamento
do programa.

Exemplo correto:
```Java
interface Flyable
    {
        void fly();
        // Outros métodos relacionados a voar
    }

    class Bird
    {
        // Lógica comum para todas as aves
    }

    class Sparrow extends Bird implements Flyable
    {
        @Override
        public void fly()
        {
            // Lógica para voar
        }
    }

    class Ostrich extends Bird
    {
        // Lógica para avestruzes
    }
```
Para corrigir o exemplo anterior, podemos usar uma interface Flyable com o método fly() e fazer com que a classe
Bird implemente essa interface. Dessa forma, a classe Sparrow pode substituir a classe Bird sem afetar o
comportamento do programa.

Além disso, a classe Ostrich não precisa implementar a interface Flyable, pois as avestruzes não voam.

### Interface Segregation Principle
Princípio da Segregação de Interfaces (ISP)
O ISP preconiza que uma classe não deve ser forçada a implementar interfaces que não utiliza.
Em vez disso, devem ser criadas interfaces mais específicas para cada contexto.

Exemplo incorreto:
```Java
interface WorkerAndEater
    {
        void work();
        void eat();
    }

    class Human implements WorkerAndEater
    {
        // Implementações para work() e eat()
    }

    class Robot implements WorkerAndEater
    {
        // Implementações fictícias para work() e eat()
    }
```
Aqui temos uma interface WorkerAndEater que contém os métodos work() e eat().
A classe Human implementa essa interface, mas não precisa do método work().
Isso viola o princípio ISP, pois a classe Human é forçada a implementar um método que não utiliza.

Exemplo correto:
```Java
interface Worker
    {
        void work();
    }

    interface Eater
    {
        void eat();
    }

    class Human implements Worker, Eater
    {
        // Implementações para work() e eat()
    }

    class Robot implements Worker
    {
        // Implementação para work()
    }
```
Para corrigir o exemplo anterior, podemos criar interfaces separadas para cada responsabilidade.
Dessa forma, a classe Human implementa apenas a interface Worker e a classe Robot implementa apenas a interface Eater.

### Dependency Inversion Principle
Princípio da Inversão de Dependência (DIP)
O DIP sugere que módulos de alto nível não devem depender diretamente de módulos de baixo nível.
Ambos devem depender de abstrações. Além disso, detalhes devem depender de abstrações, não o contrário.

Exemplo incorreto:
```Java
class EmailSender
    {
        public void sendEmail(String message)
        {
            // Lógica para enviar email
        }
    }

    class NotificationService
    {
        private final EmailSender emailSender;

        public NotificationService(EmailSender emailSender)
        {
            this.emailSender = emailSender;
        }

        public void sendNotification(String message)
        {
            emailSender.sendEmail(message);
        }
    }
```
Aqui temos uma classe NotificationService que depende diretamente da classe EmailSender.
Isso viola o princípio DIP, pois a classe de alto nível (NotificationService) depende de uma classe de baixo nível (EmailSender).

Exemplo correto:
```Java
interface MessageSender
    {
        void sendMessage(String message);
    }

    class EmailSender implements MessageSender
    {
        @Override
        public void sendMessage(String message)
        {
            // Lógica para enviar email
        }
    }

    class NotificationService
    {
        private final MessageSender messageSender;

        public NotificationService(MessageSender messageSender)
        {
            this.messageSender = messageSender;
        }

        public void sendNotification(String message)
        {
            messageSender.sendMessage(message);
        }
    }
```
Para corrigir o exemplo anterior, podemos criar uma interface MessageSender que define um método sendMessage().
Em seguida, a classe EmailSender implementa essa interface e a classe NotificationService depende da interface MessageSender.
Dessa forma, a classe de alto nível (NotificationService) depende de uma abstração (MessageSender) em vez de uma classe concreta.