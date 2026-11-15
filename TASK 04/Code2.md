@Override

public void mouseClicked(MouseEvent e) {

    points.add(new Point(e.getX(), e.getY()));

    status.setText("Clicks: " + points.size());

    repaint();
}
