# mongo_with_ansible

**MongoDB Cluster Automation with Ansible**

**Автоматизированное развертывание MongoDB на наименее загруженном сервере с настройкой сети и ограничением доступа.**

---

## **Оглавление**

1. [**Описание**](#описание)
2. [**Требования**](#требования)
3. [**Быстрый старт**](#быстрый-старт)
4. [**Как это работает**](#как-это-работает)
5. [**Проверка работы**](#проверка-работы)

---

## **Описание**

Этот проект автоматизирует:  
**Развертывание MongoDB** на наименее загруженном сервере (Alt Linux / Astra Linux)  
**Настройку сети**:

- Самый загруженный сервер становится **маршрутизатором**
- Доступ в интернет **через самый загруженный сервер**  
  **Ограничение доступа**:
- MongoDB доступен **только с RedOS**
- Остальные серверы **не могут подключиться**
  **Доступ по имени** (`mongodb.local`)

---

## **Требования**

- **3 сервера**:
  - 1 × Alt Linux
  - 2 × Astra Linux
  - 1 × RedOS
- **Ansible** (управляющая машина)
- **SSH-доступ** с правами sudo

---

## **Быстрый старт**

### **1. Клонирование и настройка**

```bash
git clone https://github.com/yourusername/mongodb-ansible-automation.git
cd mongodb-ansible-automation
```

### **2. Настройка инвентаря**

Отредактируйте `inventory/hosts.ini` заменив ip адреса серверов и (или) добавив нужные сервера:

```ini
[all_servers]
alt_linux ansible_host=your_host ansible_user=admin ansible_ssh_pass=password
astra_linux1 ansible_host=your_host ansible_user=admin ansible_ssh_pass=password
redos ansible_host=your_host ansible_user=admin ansible_ssh_pass=password
```

### **3. Запуск автоматизации**

```bash
# Выбор серверов по нагрузке
ansible-playbook -i inventory/hosts.ini playbooks/01-select-servers.yml

# Установка MongoDB
ansible-playbook -i inventory/hosts.ini playbooks/02-deploy-mongodb.yml

# Настройка сети
ansible-playbook -i inventory/hosts.ini playbooks/03-configure-network.yml

# Проверка доступа
ansible-playbook -i inventory/hosts.ini playbooks/04-verify-access.yml
```

---

## **Проверка работы**

### **1. Подключение к MongoDB с RedOS**

```bash
ssh admin@redos
mongo mongodb.local --username admin --password securepassword123 --authenticationDatabase admin
```

**Должно подключиться успешно.**

### **2. Попытка подключения с других серверов**

```bash
ssh admin@alt_linux
mongo mongodb.local --username admin --password securepassword123 --authenticationDatabase admin
```

**Должно быть отказано в доступе.**
