Głównie jest tutaj mówione o natywnym fetch korzystanym w Node.js i o bibliotece undici, która ma Pool czy stream do używania gdy zwykły fetch ogranicza wydajność i potrzebne są niskopoziomowe zarządzania połączeniem TCP.

Natomiast aktualnym standardem na frontendzie jest:
- natywny fetch
- Axios
- TanStack Query (React Query) / SWR + (fetch lub Axios)
  
A na backendzie:
- proste integracje zewnętrzne - fetch
- praca z bazadami danych to ORM-y
- gotowe usługi mają SDK
- komunikacja między mikroserwisami to gRPC, Kafka/RabbitMQ lub Axios