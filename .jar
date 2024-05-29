import javax.sound.sampled.*;
import javax.swing.*;
import java.awt.*;
import java.io.File;
import java.io.IOException;

public class CursorLockAndAudioLoop {

    private static Robot robot;
    private static boolean locked = false;
    private static Point lockPoint;
    private static Clip audioClip;

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            JFrame frame = new JFrame("Cursor Lock and Audio Loop");
            frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            frame.setSize(300, 200);
            frame.setLocationRelativeTo(null);

            JButton lockButton = new JButton("Lock Cursor");
            lockButton.addActionListener(e -> lockCursor());

            JButton unlockButton = new JButton("Unlock Cursor");
            unlockButton.addActionListener(e -> unlockCursor());

            JButton playAudioButton = new JButton("Play Audio");
            playAudioButton.addActionListener(e -> playAudioInLoop("path/to/your/audiofile.wav"));

            frame.getContentPane().setLayout(new FlowLayout());
            frame.getContentPane().add(lockButton);
            frame.getContentPane().add(unlockButton);
            frame.getContentPane().add(playAudioButton);
            frame.setVisible(true);
        });

        try {
            robot = new Robot();
        } catch (AWTException e) {
            e.printStackTrace();
        }
    }

    private static void lockCursor() {
        if (locked) return;

        lockPoint = MouseInfo.getPointerInfo().getLocation();
        locked = true;

        new Thread(() -> {
            while (locked) {
                robot.mouseMove(lockPoint.x, lockPoint.y);
                try {
                    Thread.sleep(50); // Adjust the sleep time as necessary
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }).start();
    }

    private static void unlockCursor() {
        locked = false;
    }

    private static void playAudioInLoop(String filePath) {
        try {
            if (audioClip != null && audioClip.isOpen()) {
                audioClip.close();
            }

            File audioFile = new File(filePath);
            AudioInputStream audioStream = AudioSystem.getAudioInputStream(audioFile);
            audioClip = AudioSystem.getClip();
            audioClip.open(audioStream);

            audioClip.addLineListener(event -> {
                if (event.getType() == LineEvent.Type.STOP && audioClip.getFramePosition() == audioClip.getFrameLength()) {
                    audioClip.setFramePosition(0); // Rewind to the beginning
                    audioClip.start();
                }
            });

            audioClip.start(); // Start playing the audio

        } catch (UnsupportedAudioFileException | IOException | LineUnavailableException e) {
            e.printStackTrace();
        }
    }
}
