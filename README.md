- 👋 Hi, I’m @HadziqAlaydrus
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
HadziqAlaydrus/HadziqAlaydrus is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->



const express = require('express');
const pool = require('../db/db');
const router = express.Router();

// Get all appetizers
router.get('/appetizer', async (req, res) => {
    try {
        const result = await pool.query('SELECT * FROM appetizer');
        res.json(result.rows);
    } catch (err) {
        console.error(err);
        res.status(500).json({ error: 'Failed to get appetizers' });
    }
});

// Get all main dishes
router.get('/makanan_utama', async (req, res) => {
    try {
        const result = await pool.query('SELECT * FROM makanan_utama');
        res.json(result.rows);
    } catch (err) {
        console.error(err);
        res.status(500).json({ error: 'Failed to get main dishes' });
    }
});

// Get all drinks
router.get('/minuman', async (req, res) => {
    try {
        const result = await pool.query('SELECT * FROM minuman');
        res.json(result.rows);
    } catch (err) {
        console.error(err);
        res.status(500).json({ error: 'Failed to get drinks' });
    }
});

module.exports = router;


const express = require('express');
const pool = require('../db/db');
const router = express.Router();

// Create a new order
router.post('/order', async (req, res) => {
    const { konfirmasi_id } = req.body;
    try {
        const result = await pool.query(
            'INSERT INTO "order" (konfirmasi_id) VALUES ($1) RETURNING *',
            [konfirmasi_id]
        );
        res.json(result.rows[0]);
    } catch (err) {
        console.error(err);
        res.status(500).json({ error: 'Failed to create order' });
    }
});

// Create a new konfirmasi
router.post('/konfirmasi', async (req, res) => {
    const { nomor_meja, nama } = req.body;
    try {
        const result = await pool.query(
            'INSERT INTO konfirmasi (nomor_meja, nama) VALUES ($1, $2) RETURNING *',
            [nomor_meja, nama]
        );
        res.json(result.rows[0]);
    } catch (err) {
        console.error(err);
        res.status(500).json({ error: 'Failed to create konfirmasi' });
    }
});

module.exports = router;



const express = require('express');
const bodyParser = require('body-parser');
const cors = require('cors');
const menuRoutes = require('./routes/menu');
const orderRoutes = require('./routes/order');

const app = express();
const PORT = 3000;

// Middleware
app.use(cors());
app.use(bodyParser.json());

// Routes
app.use('/api/menu', menuRoutes);
app.use('/api', orderRoutes);

// Start server
app.listen(PORT, () => {
    console.log(`Server running on http://localhost:${PORT}`);
});
