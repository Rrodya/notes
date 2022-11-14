# Pinia

## Создание

Pinia - библиотека для работы с глобальным стейтом

Параллельно с разработкой 5-й версии Vuex, создавалась Pinia. Из-за некоторых архитектурных решений pinia стало предлогаться как рекомендуемая библиотека для глобального хранилища

## Основные отличие Pinia от Vuex

1. Работает на основе моделей, легче обращаться из компонентов, можно вызывать один модуль из другого
2. Отсутсвует мутации, во vuex для синхронных операция (изменения переменных в стейте) если обратиться к серверу то имспользовали actions. В Pinia для все операция используем actions
3. Из коробки отличная поддержка TypeScript
4. Можно использовать синтаксис, окторый очень похож на Composition API
5. 