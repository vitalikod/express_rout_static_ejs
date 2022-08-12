const express = require('express');
const server = express();

server.get('/', function (req, res) {
  res.send('Hello World')
});

server.get('/1', function (req, res) {
    res.send('World')
});

server.get('/12', function (req, res) {
  res.send('hello')
});

server.get('/article/:id', (req, res) => {
    const { id } = req.params;    // const id = req.params.id;
    res.send(`this is article id ${id}`);
});

// server.get('/:cat/:name', (req, res) => {
//   const { cat, name} = req.params;
//   const sum = Number(cat) + Number(name);
//   res.send(`sum: ${sum}`);
//   // res.send(`this is category ${cat} and product ${name}`);
// });

const cars = [
  {name: 'opel', age: 9},
  {name: 'toyota', age: 14},
  {name: 'mazda', age: 8},
  {name: 'ford', age: 10}
];
const jjj = JSON.stringify(cars);

server.get('/cars', (req, res) => {
  res.send(jjj);
});
// static file ==================================
server.use(express.static('./img'));
//shablonizator ==============================
server.set('view engine', 'ejs');
server.set('views', './views');
const getInfo = (id) => { 
  const dataBase = [
  {id: 1, title: 'машина форд', content: 'мне не нравятся форды'},
  {id: 2, title: 'машина тойота', content: 'мне очень нравится тойота'},
  {id: 3, title: 'машина мазда', content: 'мне не сильно но ездить можно'},

  ];
  const result = dataBase.find(item => item.id === id);
  return result;
};
server.get('/shablon/:id', (req, res) => {
  const { id } = req.params;
  const shablon = getInfo( Number(id) );
  const arcticle = { title: shablon.title, content: shablon.content };
  res.render('test', arcticle);
});
server.listen(3000);