- 👋 Hi, I’m @raffipoghosya
- 👀 I’m interested in codeng
- 🌱 I’m currently learning react.js
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me in instagram _raffipoghosyan

function export_posts_page() {
    ?>
    <div class="wrap">
        <h2>Export Posts</h2>
        <form method="post" action="">
            <label for="start_date">Start Date:</label>
            <input type="date" id="start_date" name="start_date" required><br><br>

            <label for="end_date">End Date:</label>
            <input type="date" id="end_date" name="end_date" required><br><br>

            <label for="categories">Categories:</label>
            <select id="categories" name="categories[]" multiple required>
                <?php
                $categories = get_categories();
                foreach ($categories as $category) {
                    echo "<option value='{$category->term_id}'>{$category->name}</option>";
                }
                ?>
            </select><br><br>

            <input type="submit" name="export_posts" value="Export to CSV">
        </form>
    </div>
    <?php
}
