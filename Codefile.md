# Downloadable-Resume-Generator
import java.io.FileOutputStream;
import java.text.SimpleDateFormat;
import java.util.Date;

import com.itextpdf.text.Document;
import com.itextpdf.text.Paragraph;
import com.itextpdf.text.pdf.PdfWriter;

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class ResumeGenerator {

    private static JFrame frame;
    private static JTextField nameField;
    private static JTextArea experienceTextArea;

    public static void main(String[] args) {
        frame = new JFrame("Resume Generator");
        frame.setSize(400, 300);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        JPanel panel = new JPanel();
        panel.setLayout(new BorderLayout());

        JLabel nameLabel = new JLabel("Name:");
        nameField = new JTextField();

        JLabel experienceLabel = new JLabel("Experience:");
        experienceTextArea = new JTextArea();

        JButton generateButton = new JButton("Generate Resume");

        panel.add(nameLabel, BorderLayout.NORTH);
        panel.add(nameField, BorderLayout.NORTH);
        panel.add(experienceLabel, BorderLayout.CENTER);
        panel.add(new JScrollPane(experienceTextArea), BorderLayout.CENTER);
        panel.add(generateButton, BorderLayout.SOUTH);

        frame.add(panel);
        frame.setVisible(true);

        generateButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                generateResumePDF();
            }
        });
    }

    private static void generateResumePDF() {
        Document document = new Document();

        try {
            JFileChooser fileChooser = new JFileChooser();
            int saveDialogResult = fileChooser.showSaveDialog(frame);

            if (saveDialogResult == JFileChooser.APPROVE_OPTION) {
                String savePath = fileChooser.getSelectedFile().getAbsolutePath();
                PdfWriter.getInstance(document, new FileOutputStream(savePath + ".pdf"));
                document.open();

                String name = nameField.getText();
                String experience = experienceTextArea.getText();

                document.add(new Paragraph("Resume for: " + name));
                document.add(new Paragraph("Generated on: " + new SimpleDateFormat("dd-MM-yyyy HH:mm:ss").format(new Date())));
                document.add(new Paragraph("Experience:\n" + experience));

                JOptionPane.showMessageDialog(frame, "Resume generated successfully!");
            } else {
                JOptionPane.showMessageDialog(frame, "Resume generation canceled.");
            }

        } catch (Exception e) {
            e.printStackTrace();
            JOptionPane.showMessageDialog(frame, "Error generating PDF.");
        } finally {
            document.close();
        }
    }
}
