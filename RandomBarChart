import javax.swing.*;//for window and button
import java.awt.*;//for graphics
import java.awt.event.ActionEvent;//for redraw button click
import java.awt.geom.Line2D;//for drawing lines
import java.util.ArrayList;
import java.util.List;
import java.util.Random;

public class RandomBarChart {

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            JFrame frame = new JFrame("Random Bar Chart");//make the window
            frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

            DrawingPanel drawingPanel = new DrawingPanel();//make the grid

            JButton redrawBtn = new JButton(new AbstractAction("Redraw") {//redraw button function
                public void actionPerformed(ActionEvent e) {//on button click
                    drawingPanel.regenerateLines();//make new lines
                    drawingPanel.repaint();//redraw new lines
                }
            });

            //drawing panel placement
            frame.setLayout(new BorderLayout());
            frame.add(drawingPanel, BorderLayout.CENTER);

            //redraw button placement
            JPanel bottom = new JPanel(new FlowLayout(FlowLayout.CENTER));
            bottom.add(redrawBtn);
            frame.add(bottom, BorderLayout.SOUTH);

            //set size, puts in middle and shows window
            frame.setSize(520, 560);
            frame.setLocationRelativeTo(null);
            frame.setVisible(true);
        });
    }

    static class DrawingPanel extends JPanel {
        private static final int GRID = 10;// 10x10 grid
        private static final float THICKNESS = 10f;

        private final Random rng = new Random();//rng for bars

        private static class ColoredLine {//bar class
            final Line2D.Double line;//shape of line
            final Color color;//color

            ColoredLine(Line2D.Double line, Color color) {
                this.line = line;
                this.color = color;
            }
        }

        private final List<ColoredLine> lines = new ArrayList<>();//holds bars on screen

        DrawingPanel() {
            setBackground(new Color(200, 200, 200));//grey background
            regenerateLines();
        }

        void regenerateLines() {//creates bars
            lines.clear();//clear all lines
            for (int col = 0; col < GRID; col++) {//go through all bars
                int heightCells = 1 + rng.nextInt(GRID);//random height
                double xCell = col + 0.5;//place in middle of grid square
                double y1Cell = GRID;//starts at bottom
                double y2Cell = GRID - heightCells;//for bar height

                lines.add(new ColoredLine(
                        new Line2D.Double(xCell, y1Cell, xCell, y2Cell),//create line with height
                        new Color(rng.nextInt(256), rng.nextInt(256), rng.nextInt(256))//random color
                ));
            }
        }

        protected void paintComponent(Graphics g) {
            super.paintComponent(g);//clear panel

            Graphics2D g2 = (Graphics2D) g.create();
            try {
                g2.setRenderingHint(RenderingHints.KEY_ANTIALIASING, RenderingHints.VALUE_ANTIALIAS_ON);

                int w = getWidth();//for drawing area
                int h = getHeight();

                //makes grid square
                int margin = 30;
                int size = Math.min(w, h) - 2 * margin;
                if (size <= 0) return;
                int left = (w - size) / 2;
                int top = (h - size) / 2;
                double cell = size / (double) GRID;

                // Draw grid (10x10)
                g2.setColor(new Color(80, 80, 80));
                g2.setStroke(new BasicStroke(1f));

                for (int i = 0; i <= GRID; i++) {
                    int x = (int) Math.round(left + i * cell);
                    int y = (int) Math.round(top + i * cell);

                    g2.drawLine(x, top, x, top + size);//vertical line
                    g2.drawLine(left, y, left + size, y);//horizontal line
                }

                g2.setStroke(new BasicStroke(THICKNESS, BasicStroke.CAP_BUTT, BasicStroke.JOIN_MITER));//draw bar

                for (ColoredLine cl : lines) {//for every bar
                    g2.setColor(cl.color);

                    double x1 = left + cl.line.x1 * cell;//convert to pixels
                    double y1 = top + cl.line.y1 * cell;
                    double x2 = left + cl.line.x2 * cell;
                    double y2 = top + cl.line.y2 * cell;

                    g2.draw(new Line2D.Double(x1, y1, x2, y2));//draw
                }
            } finally {
                g2.dispose();
            }
        }
    }
}

