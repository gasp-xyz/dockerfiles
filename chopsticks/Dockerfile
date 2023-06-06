FROM --platform=amd64 node:18-alpine

WORKDIR /app

RUN yarn add @acala-network/chopsticks@0.6.5

COPY . .

CMD ["npx", "@acala-network/chopsticks", "xcm", \
  "-r", "/app/configs/kusama.yml", \
  "-p", "/app/configs/mangata.yml", \
  "-p", "/app/configs/bifrost.yml", \
  "-p", "/app/configs/turing.yml", \
  "-p", "/app/configs/imbue.yml", \
  "-p", "/app/configs/statemine.yml" \
]