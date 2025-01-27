- 👋 Hi, I’m @raffipoghosya
- 👀 I’m interested in codeng
- 🌱 I’m currently learning react.js
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me in instagram _raffipoghosyan

function handle_export_posts() {
    if (isset($_POST['export_posts'])) {
        $start_date = sanitize_text_field($_POST['start_date']);
        $end_date = sanitize_text_field($_POST['end_date']);
        $categories = isset($_POST['categories']) ? $_POST['categories'] : [];

        $args = [
            'date_query' => [
                [
                    'after' => $start_date,
                    'before' => $end_date,
                    'inclusive' => true,
                ],
            ],
            'category__in' => $categories,
            'posts_per_page' => -1,
        ];

        $query = new WP_Query($args);

        if ($query->have_posts()) {
            $csv_output = "Title,Date,Category\n";
            while ($query->have_posts()) {
                $query->the_post();
                $post_title = get_the_title();
                $post_date = get_the_date();
                $categories = get_the_category();
                $category_names = implode(', ', wp_list_pluck($categories, 'name'));

                $csv_output .= "\"$post_title\",\"$post_date\",\"$category_names\"\n";
            }
            wp_reset_postdata();

            header('Content-Type: text/csv');
            header('Content-Disposition: attachment; filename="exported_posts.csv"');
            echo $csv_output;
            exit;
        } else {
            echo '<p>No posts found for the selected criteria.</p>';
        }
    }
}
add_action('admin_init', 'handle_export_posts');
