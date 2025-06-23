FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

ENV REACT_APP_API_URL=http://backend:5050

EXPOSE 5173

CMD ["npm", "run", "dev", "--", "--host", "--port", "5173"]
