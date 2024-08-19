# day-42-task
app.post('/shorten-url', async (req, res) => {
    const { original_url } = req.body;
    const short_url = generateShortUrl(); // Implement URL generation logic
    await db.collection('urls').insertOne({
        original_url,
        short_url,
        created_at: new Date(),
        click_count: 0
    });
    res.json({ short_url });
});
app.get('/:short_url', async (req, res) => {
    const { short_url } = req.params;
    const urlData = await db.collection('urls').findOne({ short_url });
    if (urlData) {
        await db.collection('urls').updateOne(
            { short_url },
            { $inc: { click_count: 1 } }
        );
        res.redirect(urlData.original_url);
    } else {
        res.status(404).send('URL not found');
    }
});
function Registration() {
    const [username, setUsername] = useState('');
    const [password, setPassword] = useState('');

    const handleSubmit = async () => {
        const response = await fetch('/register', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ username, password })
        });
        const result = await response.json();
        alert(result.message);
    };

    return (
        <form onSubmit={handleSubmit}>
            <input value={username} onChange={e => setUsername(e.target.value)} placeholder="Username" />
            <input type="password" value={password} onChange={e => setPassword(e.target.value)} placeholder="Password" />
            <button type="submit">Register</button>
        </form>
    );
}
