FROM node:12.2.0 as builder

# install chrome for protractor tests
RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add -
RUN sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list'
RUN apt-get update && apt-get install -yq google-chrome-stable

WORKDIR '/app'

ENV PATH /app/node_modules/.bin:$PATH

RUN npm install -g @angular/cli@7.3.9

COPY ./package.json ./
RUN npm install
COPY . .

# run tests
#RUN ng test --watch=false
# RUN ng e2e --port 4202

# generate build
RUN npm run build-locale --output-path=dist --prod

FROM nginx:alpine
EXPOSE 3000
COPY ./nginx/default.conf /etc/nginx/conf.d/default.conf
COPY --from=builder /app/dist /usr/share/nginx/html
CMD ["nginx", "-g", "daemon off;"]