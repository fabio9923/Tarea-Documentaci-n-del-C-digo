# SLList.h

```c++
#ifndef SLLLIST_H
#define SLLLIST_H

#include <iostream>
#include <utility>

template<typename Object>
class SLList {
public:
    struct Node {
        Object data;
        Node *next;

        Node(const Object &d = Object{}, Node *n = nullptr);

        Node(Object &&d, Node *n = nullptr);
    };

public:
    class iterator {
    public:
        iterator();

        Object &operator*();

        iterator &operator++();

        const iterator operator++(int);

        bool operator==(const iterator &rhs) const;

        bool operator!=(const iterator &rhs) const;

    private:
        Node *current;

        iterator(Node *p);

        friend class SLList<Object>;
    };

public:
    SLList();

    SLList(std::initializer_list <Object> init_list);

    ~SLList();

    iterator begin();

    iterator end();

    int size() const;

    bool empty() const;

    void clear();

    Object &front();

    void push_front(const Object &x);

    void push_front(Object &&x);

    void pop_front();

    iterator insert(iterator itr, const Object &x);

    iterator insert(iterator itr, Object &&x);

    iterator erase(iterator itr);

    void print();

private:
    Node *head;
    Node *tail;
    int theSize;

    void init();
};

#include "SLList.cpp"

#endif

```

```c++
template<typename Object>//plantilla
class SLList {//declaracion de la clase
public:// 
    struct Node {//definicion de la estructura  Node para los nodos de la lista 
        Object data;//capo de datos del nodo
        Node *next;//puntero al siguiente nodo 

        Node(const Object &d = Object{}, Node *n = nullptr);//constructor predeterminado para data y next

        Node(Object &&d, Node *n = nullptr);//constructor movimiento data
    };
```

###### Se define una plantilla para la clase SSList que se le puede especificar cualquier tipo de dato en un futuro, se define una estructura para los nodos de la lista cada nodo contiene un campo de datos y un puntero al siguiente nodo y tiene un constructor con un valor predeterminado para data y next y un constructor de movimiento para data y todo esto está público.

```c++
public:
    class iterator {//definicion de la clase iterator
    public:
        iterator();//constructor

        Object &operator*();//sobrecarga desreferencia

        iterator &operator++();//sobrecarga preincremento 

        const iterator operator++(int);//sobrecarga post incremento 

        bool operator==(const iterator &rhs) const;//sobrecarga igualdad

        bool operator!=(const iterator &rhs) const;//sobrecarga desigualdad

    private:
        Node *current;//puntero nodo actual

        iterator(Node *p);//constructor con un nodo como parametro 

        friend class SLList<Object>;//declaracion  clase SLList como amiga 
    };

```

###### Esta clase tiene su constructor y varias sobrecargas de operadores que se encargan de desreferenciación, preincremento , postincremento, igualdad y desigualdad en privado tiene un puntero al nodo actual, el constructor  con un nodo y puntero al parámetro y se declara como amigo la clase SLList. 

```c++
public:
    SLList();//constructor

    SLList(std::initializer_list <Object> init_list);//constructor lista de inicialzacion 

    ~SLList();//destructor

    iterator begin();//metodo obtener iterador inicial de la lista

    iterator end();//metodo para obtener iterador final de la lista 

    int size() const;//metodo tamaño de lista

    bool empty() const;//metodo para checar si esta bacia la lista

    void clear();//metodo para limpiar la lista

    Object &front();//metodo obterner referencia primer elemento de la lista

    void push_front(const Object &x);//metodo agregar elemento al inicio de la lista

    void push_front(Object &&x);//metodo agregar elemento al principio de la lista movimiento

    void pop_front();//metodo eliminar primer elemento de la lista 

    iterator insert(iterator itr, const Object &x);//metodo para insertar antes de una pocicion dada

    iterator insert(iterator itr, Object &&x);//metodo para insertar un elemento antes de una pocicion dada usando movimiento 

    iterator erase(iterator itr);//metodo eliminar

    void print();//metodo imprimir lista

```

###### Los anteriores son constructores, destructores y metodos publicos de la clase SLList..

```c++
private:
    Node *head;//puntero primer nodo
    Node *tail;//puntero ultimo nodo
    int theSize;//tamaño de la lista

    void init();//metodo para inicializar la lista
};

#include "SLList.cpp"//inclucion del archivo SLList.cpp

```

# Sllist.cpp

```c++
#include "SLList.h"

template<typename Object>
SLList<Object>::Node::Node(const Object &d, Node *n)
        : data{d}, next{n} {}

template<typename Object>
SLList<Object>::Node::Node(Object &&d, Node *n)
        : data{std::move(d)}, next{n} {}

template<typename Object>
SLList<Object>::iterator::iterator() : current{nullptr} {}

template<typename Object>
Object &SLList<Object>::iterator::operator*() {
    if(current == nullptr)
        throw std::logic_error("Trying to dereference a null pointer.");
    return current->data;
}

template<typename Object>
typename SLList<Object>::iterator &SLList<Object>::iterator::operator++() {
    if(current)
        current = current->next;
    else
        throw std::logic_error("Trying to increment past the end.");
    return *this;
}

template<typename Object>
typename SLList<Object>::iterator SLList<Object>::iterator::operator++(int) {
    iterator old = *this;
    ++(*this);
    return old;
}

template<typename Object>
bool SLList<Object>::iterator::operator==(const iterator &rhs) const {
    return current == rhs.current;
}

template<typename Object>
bool SLList<Object>::iterator::operator!=(const iterator &rhs) const {
    return !(*this == rhs);
}

template<typename Object>
SLList<Object>::iterator::iterator(Node *p) : current{p} {}

template<typename Object>
SLList<Object>::SLList() : head(new Node()), tail(new Node()), theSize(0) {
    head->next = tail;
}

template<typename Object>
SLList<Object>::SLList(std::initializer_list <Object> init_list) {
    head = new Node();
    tail = new Node();
    head->next = tail;
    theSize = 0;
    for(const auto& x : init_list) {
        push_front(x);
    }
}

template<typename Object>
SLList<Object>::~SLList() {
    clear();
    delete head;
    delete tail;
}

template<typename Object>
typename SLList<Object>::iterator SLList<Object>::begin() { return {head->next}; }

template<typename Object>
typename SLList<Object>::iterator SLList<Object>::end() { return {tail}; }

template<typename Object>
int SLList<Object>::size() const { return theSize; }

template<typename Object>
bool SLList<Object>::empty() const { return size() == 0; }

template<typename Object>
void SLList<Object>::clear() { while (!empty()) pop_front(); }

template<typename Object>
Object &SLList<Object>::front() {
    if(empty())
        throw std::logic_error("List is empty.");
    return *begin();
}

template<typename Object>
void SLList<Object>::push_front(const Object &x) { insert(begin(), x); }

template<typename Object>
void SLList<Object>::push_front(Object &&x) { insert(begin(), std::move(x)); }

template<typename Object>
void SLList<Object>::pop_front() {
    if(empty())
        throw std::logic_error("List is empty.");
    erase(begin());
}

template<typename Object>
typename SLList<Object>::iterator SLList<Object>::insert(iterator itr, const Object &x) {
    Node *p = itr.current;
    Node *newNode = new Node{x, p->next};
    p->next = newNode;
    theSize++;
    return iterator(newNode);
}

template<typename Object>
typename SLList<Object>::iterator SLList<Object>::insert(iterator itr, Object &&x) {
    Node *p = itr.current;
    Node *newNode = new Node{std::move(x), p->next};
    p->next = newNode;
    theSize++;
    return iterator(newNode);
}

template<typename Object>
typename SLList<Object>::iterator SLList<Object>::erase(iterator itr) {
    if (itr == end())
        throw std::logic_error("Cannot erase at end iterator");
    Node *p = head;
    while (p->next != itr.current) p = p->next;
    Node *toDelete = itr.current;
    p->next = itr.current->next;
    delete toDelete;
    theSize--;
    return iterator(p->next);
}

template<typename Object>
void SLList<Object>::print() {
    iterator itr = begin();
    while (itr != end()) {
        std::cout << *itr << " ";
        ++itr;
    }
    std::cout << std::endl;
}

template<typename Object>
void SLList<Object>::init() {
    theSize = 0;
    head->next = tail;
}
```
```c++
//constructor Node constante
template<typename Object>
SLList<Object>::Node::Node(const Object &d, Node *n)
        : data{d}, next{n} {}
//constructor Node movimiento
template<typename Object>
SLList<Object>::Node::Node(Object &&d, Node *n)
        : data{std::move(d)}, next{n} {}
//constructor iterador
template<typename Object>
SLList<Object>::iterator::iterator() : current{nullptr} {}
//desreferencia
template<typename Object>
Object &SLList<Object>::iterator::operator*() {
    if(current == nullptr)
        throw std::logic_error("Trying to dereference a null pointer.");
    return current->data;
}
//preincremento
template<typename Object>
typename SLList<Object>::iterator &SLList<Object>::iterator::operator++() {
    if(current)
        current = current->next;
    else
        throw std::logic_error("Trying to increment past the end.");
    return *this;
}
//postincremento
template<typename Object>
typename SLList<Object>::iterator SLList<Object>::iterator::operator++(int) {
    iterator old = *this;
    ++(*this);
    return old;
}
//igualdad
template<typename Object>
bool SLList<Object>::iterator::operator==(const iterator &rhs) const {
    return current == rhs.current;
}
//desigualdad
template<typename Object>
bool SLList<Object>::iterator::operator!=(const iterator &rhs) const {
    return !(*this == rhs);
}
//constructor iterador parametro
template<typename Object>
SLList<Object>::iterator::iterator(Node *p) : current{p} {}
//constructor SLList
template<typename Object>
SLList<Object>::SLList() : head(new Node()), tail(new Node()), theSize(0) {
    head->next = tail;
}
//SLList lista inicializada
template<typename Object>
SLList<Object>::SLList(std::initializer_list <Object> init_list) {
    head = new Node();
    tail = new Node();
    head->next = tail;
    theSize = 0;
    for(const auto& x : init_list) {
        push_front(x);
    }
}
//destructor
template<typename Object>
SLList<Object>::~SLList() {
    clear();
    delete head;
    delete tail;
}
//iterador inicio lista
template<typename Object>
typename SLList<Object>::iterator SLList<Object>::begin() { return {head->next}; }
//iterador final lista
template<typename Object>
typename SLList<Object>::iterator SLList<Object>::end() { return {tail}; }
//tamaño de la lista
template<typename Object>
int SLList<Object>::size() const { return theSize; }
//lista vacia
template<typename Object>
bool SLList<Object>::empty() const { return size() == 0; }
//limpiar lista
template<typename Object>
void SLList<Object>::clear() { while (!empty()) pop_front(); }
//referencia primer elemento
template<typename Object>
Object &SLList<Object>::front() {
    if(empty())
        throw std::logic_error("List is empty.");
    return *begin();
}
//agregar elemento al principio
template<typename Object>
void SLList<Object>::push_front(const Object &x) { insert(begin(), x); }
//agregar elemento al principio de la lista con movimiento
template<typename Object>
void SLList<Object>::push_front(Object &&x) { insert(begin(), std::move(x)); }
//eliminar primer elemento 
template<typename Object>
void SLList<Object>::pop_front() {
    if(empty())
        throw std::logic_error("List is empty.");
    erase(begin());
}
//insertar elemento antes de una pocicion
template<typename Object>
typename SLList<Object>::iterator SLList<Object>::insert(iterator itr, const Object &x) {
    Node *p = itr.current;
    Node *newNode = new Node{x, p->next};
    p->next = newNode;
    theSize++;
    return iterator(newNode);
}
//insertar elemento antes de una pocicion movimiento
template<typename Object>
typename SLList<Object>::iterator SLList<Object>::insert(iterator itr, Object &&x) {
    Node *p = itr.current;
    Node *newNode = new Node{std::move(x), p->next};
    p->next = newNode;
    theSize++;
    return iterator(newNode);
}
//eliminar elemento de una pocicion 
template<typename Object>
typename SLList<Object>::iterator SLList<Object>::erase(iterator itr) {
    if (itr == end())
        throw std::logic_error("Cannot erase at end iterator");
    Node *p = head;
    while (p->next != itr.current) p = p->next;
    Node *toDelete = itr.current;
    p->next = itr.current->next;
    delete toDelete;
    theSize--;
    return iterator(p->next);
}
//imprimir los elementos
template<typename Object>
void SLList<Object>::print() {
    iterator itr = begin();
    while (itr != end()) {
        std::cout << *itr << " ";
        ++itr;
    }
    std::cout << std::endl;
}
//inicialzar la lista 
template<typename Object>
void SLList<Object>::init() {
    theSize = 0;
    head->next = tail;
}
```

###### todo lo anterior son donde se implementan los metodos y sobrecargas para que cada uno pueda cumplir su funcion 


