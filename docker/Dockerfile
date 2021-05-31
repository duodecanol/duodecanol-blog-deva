FROM node:buster-slim as builder

WORKDIR /app

RUN apt-get update \
    && apt-get install --no-install-recommends -y git=1:2.20.1-2+deb10u3 \
    && npm -g install gatsby-cli

# add to resolve puppeteer error
RUN apt-get update \
    && apt-get install -y wget gnupg \
    && wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - \
    && sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list' \
    && apt-get update \
    && apt-get install -y google-chrome-stable fonts-ipafont-gothic fonts-wqy-zenhei fonts-thai-tlwg fonts-kacst fonts-freefont-ttf libxss1 \
      --no-install-recommends \
    && rm -rf /var/lib/apt/lists/*  

COPY package*.json ./

RUN yarn

COPY . .

RUN npm run build


FROM gatsbyjs/gatsby:latest
COPY --from=builder /app/public /pub
