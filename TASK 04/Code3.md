@Override

public void paint(Graphics g) {

    super.paint(g);

    for (Point p : points) {

        g.fillOval(
            p.x - 3,
            p.y - 3,
            6,
            6
        );
    }
}
