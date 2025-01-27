- 👋 Hi, I’m @raffipoghosya
- 👀 I’m interested in codeng
- 🌱 I’m currently learning react.js
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me in instagram _raffipoghosyan

function import_csv_to_posts($file_path) {
    if (!file_exists($file_path)) {
        die("CSV file not found.");
    }

    $csv = array_map('str_getcsv', file($file_path));

    // Extract header row
    $headers = array_shift($csv);

    foreach ($csv as $row) {
        $post_data = array_combine($headers, $row);

        // Create a new post
        $post_id = wp_insert_post([
            'post_title'   => sanitize_text_field($post_data['Title']),
            'post_content' => sanitize_textarea_field($post_data['Content']),
            'post_date'    => $post_data['Date'],
            'post_status'  => 'publish',
            'post_type'    => 'post',
        ]);

        if ($post_id && !is_wp_error($post_id)) {
            // Assign categories if provided
            if (!empty($post_data['Category'])) {
                $category_ids = explode(',', $post_data['Category']);
                wp_set_post_categories($post_id, $category_ids);
            }
            echo "Post imported successfully: " . $post_data['Title'] . "<br>";
        }
    }
}

// Usage
import_csv_to_posts('path/to/your/file.csv');
